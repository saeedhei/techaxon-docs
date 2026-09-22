# GitLab SSL Certificate Renewal

## Overview

This GitLab instance runs inside Docker using the official `gitlab/gitlab-ce` image.

GitLab is directly exposed on ports `80` and `443` and uses its built-in NGINX and Let's Encrypt integration for HTTPS.

The GitLab domain is:

```text
gitlab.techaxon.de
```

The GitLab configuration is located at:

```text
/opt/gitlab/
```

The SSL certificates are stored in:

```text
/opt/gitlab/config/ssl/
```

---

## Docker Configuration

The GitLab container is configured in:

```text
/opt/gitlab/docker-compose.yml
```

The relevant configuration is:

```yaml
environment:
  GITLAB_OMNIBUS_CONFIG: |
    external_url "https://#{ENV['DOMAIN']}"

    nginx['listen_https'] = true
    nginx['redirect_http_to_https'] = true

    letsencrypt['enable'] = true
    letsencrypt['contact_emails'] = ["#{ENV['OWNER_EMAIL']}"]
```

The `/etc/gitlab` directory inside the container is mapped to:

```text
/opt/gitlab/config
```

Therefore, certificates inside the container:

```text
/etc/gitlab/ssl/
```

are stored on the host at:

```text
/opt/gitlab/config/ssl/
```

---

## Checking the Current Certificate

To check the certificate currently used by GitLab:

```bash
sudo openssl x509 \
  -in /opt/gitlab/config/ssl/gitlab.techaxon.de.crt \
  -noout -issuer -subject -dates
```

Example:

```text
issuer=C=US, O=Let's Encrypt, CN=YR2
subject=CN=gitlab.techaxon.de
notBefore=Jun 23 21:42:52 2026 GMT
notAfter=Sep 21 21:42:51 2026 GMT
```

The `notAfter` value shows when the certificate expires.

---

## Renewing the Let's Encrypt Certificate

Go to the GitLab Docker directory:

```bash
cd /opt/gitlab
```

Run the GitLab certificate renewal command:

```bash
sudo docker compose exec gitlab gitlab-ctl renew-le-certs
```

This is the command responsible for requesting/renewing the Let's Encrypt certificate.

After renewal, verify the new certificate:

```bash
sudo openssl x509 \
  -in /opt/gitlab/config/ssl/gitlab.techaxon.de.crt \
  -noout -issuer -subject -dates
```

The `notAfter` date should now be extended.

---

## Reapplying GitLab Configuration

If GitLab reports that Let's Encrypt is not enabled, first reapply the Omnibus configuration:

```bash
cd /opt/gitlab

sudo docker compose exec gitlab gitlab-ctl reconfigure
```

Then run:

```bash
sudo docker compose exec gitlab gitlab-ctl renew-le-certs
```

The recommended sequence is therefore:

```bash
cd /opt/gitlab

sudo docker compose exec gitlab gitlab-ctl reconfigure

sudo docker compose exec gitlab gitlab-ctl renew-le-certs
```

Then verify:

```bash
sudo openssl x509 \
  -in /opt/gitlab/config/ssl/gitlab.techaxon.de.crt \
  -noout -issuer -subject -dates
```

---

## Important: Do Not Disable SSL Verification

Do **not** solve an expired certificate by disabling Git SSL verification:

```bash
git config --global http.sslVerify false
```

This only bypasses certificate validation and does not fix the actual problem.

The correct solution is to renew the GitLab certificate.

---

## Troubleshooting

### Check GitLab status

```bash
sudo docker compose exec gitlab gitlab-ctl status
```

### Check Let's Encrypt logs

```bash
sudo docker compose exec gitlab \
  bash -c "ls -lah /var/log/gitlab/lets-encrypt/ && tail -100 /var/log/gitlab/lets-encrypt/*"
```

### Check the certificate from outside the container

```bash
echo | openssl s_client \
  -connect gitlab.techaxon.de:443 \
  -servername gitlab.techaxon.de 2>/dev/null \
  | openssl x509 -noout -issuer -subject -dates
```

This verifies the certificate actually being served by GitLab over HTTPS.

### Test Git access

```bash
git ls-remote https://gitlab.techaxon.de/techaxon/techaxon.git
```

If the certificate is valid, this command should no longer return:

```text
SSL certificate problem: certificate has expired
```

---

## Current GitLab SSL Architecture

```text
Internet
   |
   | HTTPS :443
   v
gitlab.techaxon.de
   |
   v
Docker
   |
   v
gitlab/gitlab-ce
   |
   +-- Built-in NGINX
   |
   +-- Let's Encrypt
   |
   +-- SSL Certificate
       |
       v
/opt/gitlab/config/ssl/
```

Traefik is **not involved** in the GitLab HTTPS setup.

GitLab directly exposes:

```text
80:80
443:443
2222:22
5050:5050
```

---

## Quick Recovery Procedure

If the GitLab SSL certificate expires again:

```bash
cd /opt/gitlab

sudo docker compose exec gitlab gitlab-ctl reconfigure

sudo docker compose exec gitlab gitlab-ctl renew-le-certs

sudo openssl x509 \
  -in /opt/gitlab/config/ssl/gitlab.techaxon.de.crt \
  -noout -issuer -subject -dates
```

Then test:

```bash
git ls-remote https://gitlab.techaxon.de/techaxon/techaxon.git
```

If the certificate was successfully renewed, Git operations such as:

```bash
git push
git pull
git clone
```

should work normally again.
