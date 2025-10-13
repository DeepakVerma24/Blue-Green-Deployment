# 🛠️ Deployment Steps – AWS Blue–Green Deployment Model

## 1️⃣ Create EC2 Instances
- Launch **4 EC2 instances** (Red Hat).
- 2 in subnet 1a and 2 in subnet 1b.
- upload shell script that downloads template or application from internet of two versions for all the instances.
- Open inbound ports 80 (HTTP) & 443 (HTTPS)
-check whether templates are accessible through instance ip address.

2️⃣ Create Target Groups
- Go to EC2 → Target Groups → Create
- blue-tg → attach Blue EC2 instance
- green-tg → attach Green EC2 instance
- Set health check path as /index.html

3️⃣ Create Application Load Balancer (ALB)
- Type: Internet-facing
- Add both target groups
- Configure listeners:
1.Port 80 → HTTP (redirect to HTTPS)
2.Port 443 → HTTPS (with ACM certificate)
- Default target: blue-tg

4️⃣ Configure Route 53
- Create a hosted zone (your domain)
- Add an A Record → Alias → ALB DNS
- Your website is now accessible via custom domain

5️⃣ Enable SSL with AWS ACM
- Go to AWS Certificate Manager
- Request certificate for your domain
- Validate using Route 53 DNS records
- Attach the certificate to ALB listener (HTTPS 443)

6️⃣ Deploy Blue–Green Switch
- Test new (Green) environment directly
- Once verified, update ALB listener:
- Forward all traffic to green-tg
- Check DNS and verify successful switch (zero downtime)

7️⃣ Rollback (if required)
- If an issue occurs, reassign ALB listener to blue-tg
- Restore old version instantly

✅ Outcome

Successfully switched from Photography version to Purplebuzz version with zero runtime using AWS Blue–Green Deployment.

