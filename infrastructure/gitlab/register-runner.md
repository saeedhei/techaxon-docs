```
sudo docker compose run --rm gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.techaxon.de" \
  --token "GLRT_TOKEN" \
  --executor "docker" \
  --docker-image "docker:29"

sudo ls -la /opt/gitlab/runner-config
sudo cat /opt/gitlab/runner-config/config.toml

sudo docker compose up -d gitlab-runner
sudo docker ps --filter name=gitlab-runner
sudo docker logs --tail 50 gitlab-runner
```





