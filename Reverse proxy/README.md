# Reverse Proxy using Nginx

## Reverse Proxy on Ubuntu using Nginx & Deploy Jenkins on Custom Domain and Establishing SSL Configuration using OpenSSL & Certbot

### 1️. Launch EC2 Instance

- AMI: Ubuntu Server 
- Instance Type: t2.micro
- Security Group :
  
        ▪ Allow HTTPS (port 443)
        ▪ Allow HTTP (port 80)
        ▪ Allow SSH (port 22)



### 2. Install Jenkins

# Install Java (required for Jenkins)

  `sudo apt install fontconfig openjdk-17-jre -y`

# Install Jenkins

  `sudo apt install jenkins -y`

# Start and enable Jenkins

  `sudo systemctl start Jenkins`

  `sudo systemctl enable jenkins`

### 3. Install Nginx

  `sudo apt install nginx -y`
  
  `sudo systemctl enable nginx`

# Checking Statu of Nginx 

  `sudo systemctl status nginx`


### 4. Configure Listener & Routing Rules




### 5.  Test the Routing



