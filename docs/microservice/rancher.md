### rancher
集群管理

---

#### Installation
```
sudo docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /host/rancher:/var/lib/rancher \
  -v /var/log/rancher/auditlog:/var/log/auditlog \
  -e AUDIT_LEVEL=3 \
  -e AUDIT_LOG_PATH=/var/log/rancher/auditlog-api \
  -e AUDIT_LOG_MAXAGE=10 \
  -e AUDIT_LOG_MAXBACKUP=10 \
  -e AUDIT_LOG_MAXSIZE=100 \
  rancher/rancher:latest
```