# 🧹 Delete / Disable Prometheus in GitLab

## 🛑 Stop Prometheus service

```bash
sudo docker exec -it gitlab gitlab-ctl stop prometheus
🗑️ Remove Prometheus data
sudo rm -rf /opt/gitlab/data/prometheus/*
📊 Check disk usage
sudo du -sh /opt/gitlab/data/prometheus
⚙️ Disable Prometheus permanently (recommended)

Add this to docker-compose.yml:

prometheus_monitoring['enable'] = false
🔄 Restart GitLab container
sudo docker compose restart
