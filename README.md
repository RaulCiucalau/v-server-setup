# V-Server Setup with SSH & Nginx

---

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Create SSH Keys](#create-ssh-keys)
  - [First Login with Password](#first-login-with-password)
  - [Enable SSH Key Login](#enable-ssh-key-login)
  - [Disable Password Authentication](#disable-password-authentication)
- [Install Nginx](#install-nginx)
  - [Installation Process](#installation-process)
  - [Create an Alternative Website](#create-an-alternative-website)


---

## Introduction
This project explains how to securely configure a Linux V-Server using SSH key authentication and how to install and configure an Nginx web server.  
It also demonstrates how to host an alternative website on a custom port.

---

## Prerequisites
To follow this guide, you need:
- An Ubuntu-based Cloud VM or V-Server
- SSH access to the server
- A local machine with SSH installed

---

## Usage

### 1. Create SSH Keys

Generate an SSH key pair on your local machine using the recommended `ed25519` algorithm:
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

---

### 2. First Login with Password

Log in to your V-Server using your username and password:
```bash
ssh <username>@<ip-address>
```

---

### 3. Enable SSH Key Login

1.Copy your public SSH key to the V-Server.
```bash
ssh-copy-id -i ~/path/to/id_ed25519.pub <username>@<ip-address>
```

2. Test the SSH key login:
```bash
ssh -i ~/path/to/id_ed25519 <username>@<ip-address>
```

---

### 4. Disable Password Authentication

1. After confirming SSH key login works, disable password authentication.
```bash
sudo nano sshd_config
```
2. Find the following line, uncomment it, and set the value to `no`:
```bash
PasswordAuthentication no
```

3. Save and close the file.

4. Restart the SSH service:
```bash
sudo systemctl restart ssh.service
```

---

## Install Nginx

### 1. Installation Process

1. Update the package repository:
```bash
sudo apt update
```

2. Install Nginx:
```bash
sudo apt install nginx -y
```

---

### 2. Configure Nginx

1. Create a new Nginx configuration file:
```bash
sudo nano /etc/nginx/sites-enabled/alternatives
```

2. Add the following configuration:
```bash
server {
    listen 8081;
    listen [::]:8081;

    root /var/www/alternatives;
    index alternate-index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
3. Save and close the file.

---

### 3. Create an alternative HTML Page

1. Create Directory:
```bash
sudo mkdir /var/www/alternatives
```

2. Create an HTML File:
```bash
sudo touch /var/www/alternatives/alternate-index.html
```

3. Edit the file and add your HTML content:
```bash
sudo nano /var/www/alternatives/alternate-index.html
```

4. Save and close the file.

5. Restart Nginx:
```bash
sudo systemctl restart nginx
```

6. Open your browser to see your configured alternative start page:
```bash
localhost:8081
```