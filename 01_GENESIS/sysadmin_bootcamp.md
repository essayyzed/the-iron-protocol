# ☀️ Summer Break 1 – System Administration Bootcamp

> **The Missing Semester**: Servers, Docker, DevOps Fundamentals

---

## 🎯 Goals

- Acquire hands-on **systems knowledge** that universities rarely teach
- Learn to **deploy and manage real servers**
- Understand **how the internet actually works**
- Get comfortable with **production-grade tooling**

**Why This Matters:** Most CS programs teach theory but skip the practical "how do I actually deploy this?" knowledge. This summer fills that gap.

---

## 📅 8–10 Week Plan

Dedicate **3-4 hours per day**, 5 days per week.

---

## Week 1-2: Linux Server Basics

### Setup: Rent a VPS
**Providers:**
- [DigitalOcean](https://www.digitalocean.com/) – $6/month
- [Linode](https://www.linode.com/) – $5/month
- [Vultr](https://www.vultr.com/) – $5/month
- AWS Lightsail – $5/month

**OS:** Ubuntu 22.04 LTS

### What You'll Learn

#### SSH Access
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy public key to server
ssh-copy-id user@your-server-ip

# Connect
ssh user@your-server-ip
```

#### User Management
```bash
# Create new user
sudo adduser newuser

# Add to sudo group
sudo usermod -aG sudo newuser

# Switch user
su - newuser
```

#### File Permissions
```bash
# Read, write, execute
chmod 755 file.sh
chmod 644 config.txt

# Change ownership
chown user:group file.txt
```

#### Web Server Setup
```bash
# Install Nginx
sudo apt update
sudo apt install nginx

# Start service
sudo systemctl start nginx
sudo systemctl enable nginx

# Check status
sudo systemctl status nginx
```

### Week 1-2 Project
**Deploy a static website on Nginx**
- Upload your Semester 2 portfolio
- Configure Nginx to serve it
- Access via server IP

---

## Week 3-4: Docker Basics

### Why Docker?
- Consistent environments
- Easy deployment
- Isolation and security
- Industry standard

### What You'll Learn

#### Docker Basics
```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add user to docker group
sudo usermod -aG docker $USER

# Verify
docker --version
```

#### Running Containers
```bash
# Run nginx container
docker run -d -p 80:80 nginx

# List running containers
docker ps

# Stop container
docker stop container_id

# Remove container
docker rm container_id
```

#### Building Images
```dockerfile
# Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

```bash
# Build image
docker build -t myapp .

# Run image
docker run -d -p 3000:3000 myapp
```

#### Docker Compose
```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongo
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down
```

### Week 3-4 Project
**Containerize your Semester 2 CRUD app**
- Write Dockerfile for your Node.js app
- Use Docker Compose for app + MongoDB
- Deploy on your VPS

---

## Week 5-6: Automation & Scripting

### Bash Scripting

#### Basics
```bash
#!/bin/bash

# Variables
NAME="Iron Protocol"
echo "Hello, $NAME"

# Conditionals
if [ -f "file.txt" ]; then
    echo "File exists"
else
    echo "File not found"
fi

# Loops
for i in {1..5}; do
    echo "Number: $i"
done

# Functions
backup() {
    tar -czf backup-$(date +%Y%m%d).tar.gz /path/to/data
    echo "Backup completed"
}

backup
```

### Week 5-6 Projects

#### Project 1: Backup Script
```bash
#!/bin/bash
# auto-backup.sh

BACKUP_DIR="/backups"
SOURCE_DIR="/var/www/html"
DATE=$(date +%Y%m%d_%H%M%S)

tar -czf $BACKUP_DIR/backup_$DATE.tar.gz $SOURCE_DIR

# Keep only last 7 backups
cd $BACKUP_DIR
ls -t | tail -n +8 | xargs rm -f

echo "Backup completed: backup_$DATE.tar.gz"
```

#### Project 2: System Monitoring Script
```bash
#!/bin/bash
# monitor.sh

CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')
MEM=$(free -m | awk 'NR==2{printf "%.2f%%", $3*100/$2}')
DISK=$(df -h | awk '$NF=="/"{printf "%s", $5}')

echo "=== System Status ==="
echo "CPU Usage: $CPU"
echo "Memory Usage: $MEM"
echo "Disk Usage: $DISK"

# Send alert if disk > 80%
DISK_NUM=$(echo $DISK | sed 's/%//')
if [ $DISK_NUM -gt 80 ]; then
    echo "WARNING: Disk usage above 80%!"
fi
```

#### Project 3: Auto-Deployment Script
```bash
#!/bin/bash
# deploy.sh

echo "Pulling latest code..."
git pull origin main

echo "Installing dependencies..."
npm install

echo "Building application..."
npm run build

echo "Restarting services..."
docker-compose down
docker-compose up -d --build

echo "Deployment completed!"
```

### Automate with Cron
```bash
# Edit crontab
crontab -e

# Run backup daily at 2 AM
0 2 * * * /home/user/scripts/auto-backup.sh

# Run monitor every hour
0 * * * * /home/user/scripts/monitor.sh
```

---

## Week 7-8: Advanced Topics (Choose 1-2)

### Option A: Security Hardening

#### SSH Security
```bash
# Disable root login
sudo nano /etc/ssh/sshd_config
# Set: PermitRootLogin no

# Change default port
# Set: Port 2222

# Restart SSH
sudo systemctl restart sshd
```

#### Firewall (UFW)
```bash
# Enable firewall
sudo ufw enable

# Allow specific ports
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Check status
sudo ufw status
```

#### Fail2Ban (Brute-force protection)
```bash
# Install
sudo apt install fail2ban

# Configure
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

# Start
sudo systemctl start fail2ban
sudo systemctl enable fail2ban
```

---

### Option B: Monitoring & Logging

#### System Monitoring with htop
```bash
sudo apt install htop
htop
```

#### Netdata (Real-time monitoring)
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
# Access: http://your-server-ip:19999
```

#### Log Management
```bash
# View logs
sudo journalctl -xe
sudo tail -f /var/log/nginx/access.log

# Log rotation
sudo nano /etc/logrotate.d/myapp
```

---

### Option C: Reverse Proxy & SSL

#### Nginx as Reverse Proxy
```nginx
# /etc/nginx/sites-available/myapp
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### SSL with Let's Encrypt
```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d yourdomain.com

# Auto-renewal
sudo certbot renew --dry-run
```

---

## Week 9-10: Integration Project

### Final Project: Production Deployment

**Deploy your Semester 2 app with all best practices:**

1. **VPS Setup**
   - Ubuntu server
   - Non-root user
   - SSH key authentication

2. **Application**
   - Dockerized Node.js app
   - Docker Compose with MongoDB
   - Environment variables

3. **Web Server**
   - Nginx as reverse proxy
   - SSL certificate (if you have domain)

4. **Security**
   - Firewall enabled
   - Fail2Ban configured
   - Regular backups

5. **Monitoring**
   - System monitoring dashboard
   - Log rotation
   - Disk space alerts

6. **Automation**
   - Auto-backup script (daily)
   - Auto-deployment script
   - Cron jobs configured

7. **Documentation**
   - README with setup instructions
   - Architecture diagram
   - Troubleshooting guide

---

## 📦 Summer Break 1 Outputs

By the end, you should have:

- [ ] Live website/app on your own VPS
- [ ] At least one service containerized with Docker
- [ ] 3+ automation scripts (backup, deploy, monitor)
- [ ] Basic security hardening implemented
- [ ] Complete documentation (blog post or README)
- [ ] Comfort with SSH, Linux server management, and DevOps basics

---

## ✅ Success Criteria

**You're ready for Year 2 when you can:**
- SSH into a server and navigate comfortably
- Deploy a Docker container from scratch
- Write a bash script to automate a task
- Configure Nginx as a reverse proxy
- Debug server issues using logs
- Secure a Linux server against basic attacks

---

## 📖 Recommended Resources

### Courses
- [MIT Missing Semester](https://missing.csail.mit.edu/)
- [DigitalOcean Tutorials](https://www.digitalocean.com/community/tutorials)
- [Docker Official Tutorial](https://docs.docker.com/get-started/)
- [Recluze - System Administration]([https://www.youtube.com/@recluze/playlists](https://youtu.be/-42b7XFimnM?si=KtgeeD13ksftJu_F))

### Books
- *The Linux Command Line* by William Shotts
- *Docker Deep Dive* by Nigel Poulton

### YouTube
- [TechWorld with Nana](https://www.youtube.com/@TechWorldwithNana) – Docker & DevOps
- [NetworkChuck](https://www.youtube.com/@NetworkChuck) – Linux & Servers
- [LearnLinuxTV](https://www.youtube.com/@LearnLinuxTV) – Linux tutorials

---

## ⚠️ Common Mistakes

### ❌ **Skipping this summer**
"I'll learn DevOps later" = you'll graduate without deployment skills

### ❌ **Only watching tutorials**
Rent a server and break things. That's how you learn.

### ❌ **Afraid to break your server**
Snapshots exist. Break it, restore, try again.

### ❌ **Not documenting your process**
You'll forget everything. Write it down.

---

## 💡 Pro Tips

1. **Take snapshots** before major changes
2. **Keep a server journal** (commands you ran, issues you faced)
3. **Join /r/selfhosted** and /r/homelab for inspiration
4. **Consider a domain name** ($10-15/year) for SSL and professionalism
5. **Automate everything** you do more than twice

---

**Next:** [Year 2 – ARCHITECTURE (Core Computer Science)](../02_ARCHITECTURE)

---

*"Summer Break 1 is where students become engineers."*  
*— The Protocol*
