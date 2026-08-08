sudo nano /opt/gitlab/runner-config/config.toml

این:

volumes = ["/cache"]

را به این تغییر بده:

volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]

ذخیره کن.

قدم ۲ — کانتینر Runner را Restart کن

چون Runner داخل Compose است:

cd /opt/gitlab
sudo docker compose restart gitlab-runner

بعد بررسی کن:

sudo docker logs --tail 30 gitlab-runner

باید چیزی شبیه این ببینی:

Configuration loaded
Checking for jobs...
