# Reverse Proxy using Nginx

### 1️. Launch EC2 Instance

- AMI: Ubuntu Server 
- Instance Type: t2.micro
- Security Group :
    ▪ Allow HTTPS (port 443)
    ▪ Allow HTTP (port 80)
    ▪ Allow SSH (port 22) for browser terminal access

Install a web server (e.g., Nginx) on both and configure different content for each path.

![Screenshot 2025-05-26 160525](https://github.com/user-attachments/assets/0f622055-eb65-4825-9fbe-37af4e13b384)

![Screenshot 2025-05-26 160401](https://github.com/user-attachments/assets/a67e1ef1-4889-41d5-9a4f-90c532358c97)


### 2. Create Two Target Groups

Target Group A: Register the EC2 instance serving /app1

Target Group B: Register the EC2 instance serving /app2

![Screenshot 2025-05-26 160700](https://github.com/user-attachments/assets/355cfc28-a13a-464e-aca9-fdaf88984de4)


### 3. Create an Application Load Balancer

Choose Application Load Balancer, set it to internet-facing

Select at least two subnets across different availability zones

Attach a Security Group that allows inbound HTTP (port 80)

![Screenshot 2025-05-26 160817](https://github.com/user-attachments/assets/8b38d3f1-100c-4404-8156-0e7b26860e74)


### 4. Configure Listener & Routing Rules

Listener: HTTP on port 80

Rules:

- If path is /app2, forward to Target Group B

- Default rule (i.e., /app1) forwards to Target Group A

  ![Screenshot 2025-05-26 161057](https://github.com/user-attachments/assets/75163bed-a2b2-4339-986f-96dc0f8a5656)


### 5.  Test the Routing

Open a browser and verify:

http://<ALB-DNS>/app1 → displays content from Target Group A

![Screenshot 2025-05-26 161209](https://github.com/user-attachments/assets/ff35e82b-db89-47fa-a7a0-1b895b2960a2)


http://<ALB-DNS>/app2 → displays content from Target Group B

![Screenshot 2025-05-26 161225](https://github.com/user-attachments/assets/dcc42186-25a1-4f15-9486-885893de0663)

