curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" -o script.deb.sh

less script.deb.sh

sudo bash script.deb.sh

sudo apt install gitlab-runner

gitlab-runner register  --url https://gitlab.techaxon.de  --token xxxxxxxxxxxxxxxxxxxxxxxxxxxx

1) GitLab instance URL
https://gitlab.techaxon.de
2) Runner name
techaxon-production-runner
3) Executor
docker
4) Default Docker image
node:22-alpine

sudo gitlab-runner list
sudo gitlab-runner verify


