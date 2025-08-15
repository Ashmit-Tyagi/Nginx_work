# Load Balancing and Path based routing using Nginx

### 1. Launch EC2 Instance

- AMI: Ubuntu Server 
- Instance Type: t2.micro
- Security Group :
  
        ▪ Allow HTTPS (port 443)
        ▪ Allow HTTP (port 80)
        ▪ Allow SSH (port 22)

### 2. Install Jenkins

#### Install Java (required for Jenkins)

  `sudo apt install fontconfig openjdk-17-jre -y`

#### Install Jenkins

  `sudo apt install jenkins -y`

#### Start and enable Jenkins

  `sudo systemctl start Jenkins`

  `sudo systemctl enable jenkins`

### 3. Install Nginx

  `sudo apt install nginx -y`
  
  `sudo systemctl enable nginx`

#### Checking Statu of Nginx 

  `sudo systemctl status nginx`
  

### 4. Point Domain to the Server

#### In the domain registrar’s DNS settings, create an A record.

    Name: @annoyingash.icu
          www.annoyingash.icu
  
    Value: Server’s public IP

    TTL: Default

#### This points annoyingash.icu and www.annoyingash.icu to EC2 instance.


### 5. Configure Nginx as Reverse Proxy

#### Add the configuration:

  `sudo vim /etc/nginx/nginx.conf`

    server {
      listen 80;
      server_name annoyingash.icu;
  
      location / {
          proxy_pass         http://localhost:8080;
          proxy_set_header   Host $host;
          proxy_set_header   X-Real-IP $remote_addr;
          proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header   X-Forwarded-Proto $scheme;
      }
    }

#### Test and reload Nginx

  `sudo nginx -t`
  
  `sudo systemctl reload nginx`


### 6. Securing with SSL using Certbot

  #### Install Certbot

    `sudo apt install certbot python3-certbot-nginx`

  #### Run Certbot

    `sudo certbot --nginx -d annoyingash.icu`

- Follow prompts to get SSL certificates.
  
- Certbot will automatically edit the Nginx config to redirect HTTP → HTTPS


### 7. Path-Based Routing with Nginx

#### Deploy Backend Applications

      - Jenkins already running on localhost:8080.

      - Deploy another app on a different port, say localhost:5000

#### Edit Nginx Configuration

    `sudo vim /etc/nginx/nginx.conf`

    server {
      listen 80;
      server_name annoyingash.icu;
  
      location /jenkins/ {
          proxy_pass http://localhost:8080/;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  
      location /app1/ {
          proxy_pass http://localhost:5000/;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
    }


#### Test & Reload Nginx

    `sudo nginx -t`
    
    `sudo systemctl reload nginx`



