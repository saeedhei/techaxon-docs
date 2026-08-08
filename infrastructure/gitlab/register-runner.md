```
sudo docker compose run --rm gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.techaxon.de" \
  --token "GLRT_TOKEN" \
  --executor "docker" \
  --docker-image "docker:29"

sudo ls -la /opt/gitlab/runner-config
sudo cat /opt/gitlab/runner-config/config.toml
```





