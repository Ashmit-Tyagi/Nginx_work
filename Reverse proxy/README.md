# Hosting a Domain with Route 53 and Configuring Path & Host-Based Routing via ALB

Steps :

### 1.Domain Setup with Route 53

a. Buy a Domain
  - Purchase a domain from a registrar like GoDaddy, Namecheap, or Hostinger.

b. Create Hosted Zone
  - Go to the Route 53 console and create a Hosted Zone for your domain.

c. Update Name Servers
  - Copy the NS records from Route 53.
  - Go to your domain registrar's DNS settings.
  - Replace their NS records with those from Route 53.
  - Request SSL Certificate (HTTPS)

d. Go to AWS Certificate Manager (ACM).
  - Request a public certificate for both yourdomain.com and *.yourdomain.com.
  - Validate ownership by adding the CNAME record provided by ACM to your Route 53 hosted zone.



### 2. Application Setup on EC2

a. Launch EC2 Instances
  - Install Nginx or any web server.
  - Configure apps to listen on different ports (e.g., 80, 7764, 9097).

b. Create Target Groups
  - Navigate to EC2 > Target Groups.
  - Create separate target groups for each app.
  - Register corresponding EC2 instances and specify ports and health checks.

### 3. ALB Setup with Routing Rules

a. Create Application Load Balancer
  - Choose Application Load Balancer, internet-facing
  - Select 2+ Availability Zones
  - Use the same VPC as your EC2 instances
  - Attach a Security Group that allows:
    1. HTTP (port 80)
    2. HTTPS (port 443)

b. Configure Listeners
  - HTTP Listener (Port 80)
      1. Add a redirect rule to forward all HTTP traffic to HTTPS

  - HTTPS Listener (Port 443)
      1. Attach the SSL certificate from ACM

  - Add routing rules:
      1. Path-based routing: e.g., /app1 → Target Group A
      2. Host-based routing: e.g., api.yourdomain.com → Target Group A


### 4.DNS Configuration in Route 53

- Go to your domain’s hosted zone in Route 53

- Create an A Record:
  1. Record name: @ (or subdomain like api)
  2. Type: A – IPv4 address
  3. Alias: Yes
  4. Alias Target: Choose the ALB DNS name from the dropdown
 
This connects your domain to the ALB, enabling secure HTTPS access and enforcing your routing rules.

![Screenshot 2025-05-26 161335](https://github.com/user-attachments/assets/617ce8fe-d86e-4693-9364-a46bc67f7892)


### Final Result

With this setup:

  - https://yourdomain.com/app1 routes to one EC2 instance

    ![Screenshot 2025-05-26 161409](https://github.com/user-attachments/assets/f20ac675-2a41-4719-a5c9-a4156fc56109)

  - All traffic is secured via HTTPS using ACM
    
  - Domain is managed via Route 53

    ![Screenshot 2025-05-26 161438](https://github.com/user-attachments/assets/75856920-8eaa-493b-97b3-d89fafc5541c)
