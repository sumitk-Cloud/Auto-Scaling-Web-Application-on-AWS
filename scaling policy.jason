#!/bin/bash
dnf update -y
dnf install -y httpd stress-ng
systemctl enable --now httpd

# IMDSv2 token to fetch instance metadata
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id)
AZ=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/placement/availability-zone)

APP_VERSION="v1.0"

cat > /var/www/html/index.html <<EOF
<!DOCTYPE html>
<html>
<head><title>HA E-Commerce App</title></head>
<body style="font-family:Arial;text-align:center;margin-top:60px;">
  <h1>🛒 HA E-Commerce Web App</h1>
  <h2>Version: ${APP_VERSION}</h2>
  <p><b>Instance ID:</b> ${INSTANCE_ID}</p>
  <p><b>Availability Zone:</b> ${AZ}</p>
</body>
</html>
EOF

# Health check endpoint
echo "OK" > /var/www/html/health.html