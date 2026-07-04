---
title: "Deploy & systemd"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

#### Kéo JAR từ S3

```bash
sudo mkdir -p /opt/zerobug
sudo aws s3 cp s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar /opt/zerobug/app.jar
```

{{% notice info %}}
Có thể **tạm dừng** tại `mkdir` — Session Manager đóng được; lần sau Connect tiếp tục. Java/CLI vẫn còn trên EBS. **Stop EC2** tiết kiệm compute; NAT vẫn tính phí.
{{% /notice %}}

#### File `/opt/zerobug/cloud.env`

```bash
sudo tee /opt/zerobug/cloud.env > /dev/null <<'EOF'
SPRING_PROFILES_ACTIVE=cloud
AWS_REGION=ap-southeast-1
SPRING_DATASOURCE_URL=jdbc:postgresql://<RDS-ENDPOINT>:5432/zerobug
SPRING_JPA_HIBERNATE_DDL_AUTO=update
DB_SECRET_NAME=zerobug/rds/credentials
S3_BUCKET=<bucket>
EOF
```

Không hardcode password DB.

#### Systemd `/etc/systemd/system/zerobug.service`

```bash
sudo tee /etc/systemd/system/zerobug.service > /dev/null <<'EOF'
[Unit]
Description=ZeroBug Agent Spring Boot
After=network.target

[Service]
EnvironmentFile=/opt/zerobug/cloud.env
ExecStart=/usr/bin/java -jar /opt/zerobug/app.jar
Restart=always
User=root

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable --now zerobug
sudo systemctl status zerobug
sleep 20
curl http://localhost:8080/api/health
```

{{% notice warning %}}
**Chi phí:** **Stop EC2** tiết kiệm compute khi không test; **NAT Gateway vẫn tính phí ~$32/tháng** nếu chạy 24/7 — xóa khi xong đồ án (mục 5.10).
{{% /notice %}}

Log: `sudo journalctl -u zerobug -n 100`.

Khi Hoa xong: thêm `STEPFN_STATE_MACHINE_ARN=...` → `sudo systemctl restart zerobug`.

<!-- Hình: /images/5-Workshop/5.5-Toàn/systemd.png -->
