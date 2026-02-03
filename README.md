# V-Server Setup with SSH & Nginx
This project explains how to securely configure a Linux V-Server using SSH key authentication and how to install and configure an Nginx web server.  
It also demonstrates how to host an alternative website on a custom port. 
---

## Table of Contents
- [Prerequisites](#prerequisites)
- [Setup SSH Connection](#setup-ssh-connection)
  - [Create SSH Keys](#create-ssh-keys)
  - [Login with Password](#login-with-password)
  - [Enable SSH Key Login](#enable-ssh-key-login)
  - [Disable Password Authentication](#disable-password-authentication)
- [Setup Nginx](#setup-nginx)
  - [Installation Process](#installation-process)
  - [Create an Alternative Website](#create-an-alternative-website)
- [Configure Git and Github connection](#configure-git-and-github-connection)
  - [Generate the SSH Key](#generate-the-ssh-key)
  - [Grab your SSH Key](#grab-your-ssh-key)
  - [Add the Key to Github](#add-the-key-to-github)

---

## Prerequisites
To follow this guide, you need:
- An Ubuntu-based Cloud VM or V-Server
- SSH access to the server
- A local machine with SSH installed

---

## Setup SSH connection

### Create SSH Keys

Generate an SSH key pair on your local machine using the recommended `ed25519` algorithm:
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

---

### Login with Password

Log in to your V-Server using your username and password:
```bash
ssh <username>@<ip-address>
```

---

### Enable SSH Key Login

1. Copy your public SSH key to the V-Server.
```bash
ssh-copy-id -i ~/path/to/id_ed25519.pub <username>@<ip-address>
```

2. Test the SSH key login:
```bash
ssh -i ~/path/to/id_ed25519 <username>@<ip-address>
```

---

### Disable Password Authentication

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

## Setup Nginx

### Installation Process

1. Update the package repository:
```bash
sudo apt update
```

2. Install Nginx:
```bash
sudo apt install nginx -y
```

---

### Configure Nginx

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

### Create an alternative website

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

---

## Configure Git and Github connection

### Generate the SSH Key

```bash
ssh-keygen -t ed25519
```

---

### Grab your SSH Key

1. Read the key file
```bash
ssh-keygen -t ed25519
```

2. Copy the key

---

### Add the Key to Github

1. Log in to GitHub.

2. Click your Profile Photo → Settings.

3. In the left sidebar, select SSH and GPG keys.

4. Click the green New SSH key button.

5. Title: Give it a clear name (e.g., "Main-Laptop" or "Web-Server").

5. Key: Paste your public key into the box.

6. Click Add SSH key.

---