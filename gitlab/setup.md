# delete prometheus
sudo docker exec -it gitlab gitlab-ctl stop prometheus
sudo rm -rf /opt/gitlab/data/prometheus/*
sudo du -sh /opt/gitlab/data/prometheus
