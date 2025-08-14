# V-Server Setup Guide

## Table of Contents
1. [SSH Connection Setup](#1-ssh-connection-setup)  
   1.1 [Generate SSH Key](#11-generate-ssh-key)  
   1.2 [Transfer Public Key to Server](#12-transfer-public-key-to-server)  
   1.3 [Disable SSH Password Login](#13-disable-ssh-password-login)  

2. [Nginx Setup](#2-nginx-setup)  
   2.1 [Install Nginx](#21-install-nginx)  
   2.2 [Configure Alternative Landing Page](#22-configure-alternative-landing-page)  

3. [Testing](#3-testing)  

4. [Additional Notes](#4-additional-notes)  

---

## 1. SSH Connection Setup

### 1.1 Generate SSH Key
On your **local machine**, create an SSH key pair:
```bash
ssh-keygen -t ed25519
```
This will generate a public (`.pub`) and private key.

---

### 1.2 Transfer Public Key to Server
Send your public key to the server using:
```bash
ssh-copy-id -i ~/.ssh/your_key.pub user@your_server_ip
```
Replace:
- `your_key.pub` → your public key file name  
- `user@your_server_ip` → your SSH user and server IP

---

### 1.3 Disable SSH Password Login
Edit the SSH daemon configuration on your server:
```bash
sudo nano /etc/ssh/sshd_config
```
Change:
```
#PasswordAuthentication yes
```
to:
```
PasswordAuthentication no
```
Restart the SSH service:
```bash
sudo systemctl restart ssh
```

---

## 2. Nginx Setup

### 2.1 Install Nginx
Update your package list and install Nginx:
```bash
sudo apt update
sudo apt install nginx -y
```

---

### 2.2 Configure Alternative Landing Page
1. Create a directory for the alternative page:
```bash
sudo mkdir /var/www/alternative
```
2. Create the HTML file:
```bash
sudo nano /var/www/alternative/index.html
```
Add your custom HTML content and save the file.

3. Create a new server block configuration directly in the Nginx config directory (already active location):
```bash
sudo nano /etc/nginx/sites-enabled/alternative
```
Example config:
```nginx
server {
    listen 8081;
    listen [::]:8081;
    root /var/www/alternative;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
4. Test and reload Nginx:
```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 3. Testing

- **SSH Login Test (key-based)**:
```bash
ssh user@your_server_ip
```
- **SSH Password Login Disabled Test**:
```bash
ssh -o PubKeyAuthentication=no user@your_server_ip
```
Expected: login **should fail**.  

- **Webserver Test**:  
Open `http://your_server_ip:8081` in a browser and check if the alternative page appears.

---

## 4. Additional Notes
- **Security**: Never commit private SSH keys or passwords to your repository.  
- **Repository Structure**:
  - `README.md` → main documentation (this file)  
  - `Git + VServer Checkliste.pdf` → original checklist  
- **Language**: Documentation is provided in English as required.

---
