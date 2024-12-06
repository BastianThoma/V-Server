# V-Server Setup Guide

This guide will help you set up your V-Server from scratch, covering SSH configuration, password login disablement, and setting up an Nginx web server. Follow the steps below to configure your server.

---

# Table of Contents

- [1. Generate SSH Key Pair](#1-generate-ssh-key-pair)
- [2. First Login to the Server](#2-first-login-to-the-server)
- [3. Copy SSH Public Key to Server](#3-copy-ssh-public-key-to-server)
- [4. Test SSH Key Authentication](#4-test-ssh-key-authentication)
- [5. Disable Password Login](#5-disable-password-login)
- [6. Install Nginx Web Server](#6-install-nginx-web-server)
- [7. Configure an Alternative Nginx Site](#7-configure-an-alternative-nginx-site)
- [8. Create an SSH Alias](#8-create-an-ssh-alias)
- [9. Install and Configure Git](#9-install-and-configure-git)

---

## 1. **Generate SSH Key Pair**

1. Create an `ed25519` SSH key pair using the command: 
```bash
ssh-keygen -t ed25519
```
2. Save the keys in the .ssh directory (default location).
3. Verify the keys are saved with the command: 
```bash
ls ~/.ssh
```
## 2. **First Login to the Server**

1. Connect to the server using username and Server-IP with the command: 
```bash
ssh username@Server-IP
```
2. When prompted, confirm the connection by typing yes.
3. Enter your password to log in for the first time.
4. Once logged in, server information should appear.

## 3. **Copy SSH Public Key to Server**

1. Use Git Bash (or any terminal) on Windows to copy the SSH key: 
```bash
ssh-copy-id username@Server-IP
```
2. Enter your password when prompted. This will add your public key to the server.

## 4. **Test SSH Key Authentication**

1. Test logging in with your SSH key:
```bash
ssh -i ~/.ssh/id_ed25519 username@Server-IP
```
2. You should be able to log in without entering a password.

## 5. **Disable Password Login**

1. Edit the SSH configuration:
```bash
sudo nano /etc/ssh/sshd_config
```
2. Find the line:
```console
#PasswordAuthentication yes
```
3. Replace it with:
```console
PasswordAuthentication no
```
4. Save and close the editor (Ctrl+O, then Ctrl+X).
5. Restart the SSH service:
```bash
sudo systemctl restart ssh.service
```
6. Verify password login is disabled:
```bash
ssh -o PubkeyAuthentication=no username@Server-IP
```
The result should be: `Permission denied (publickey).`
## 6. Install Nginx Web Server

1. Update package lists 
```bash
sudo apt update
```
2. Install Nginx 
```bash
sudo apt install nginx -y
```
3. Verify Nginx is running 
```bash
systemctl status nginx.service
```
4. Open your Server’s IP in a Browser to check if the default Nginx page loads.

## 7. **Configure an Alternative Nginx Site**

1. Create a new directory for the site:
```bash
sudo mkdir /var/www/alternatives
```
2. Create an HTML file for the site:
```bash
sudo touch /var/www/alternatives/alternate-index.html
```
3. Configure the site:
```bash
sudo nano /etc/nginx/sites-enabled/alternatives
```
### Example configuration:
```nginx
server {
    listen 8081;
    root /var/www/alternatives;
    index alternate-index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
4. Save and close the file (Ctrl+O, then Ctrl+X).

5. Edit the HTML file:
```bash
sudo nano /var/www/alternatives/alternate-index.html
```
Customize the page as desired.
6. Restart Nginx:
```bash
sudo service nginx restart
```
7. Verify the new site:
Open your server’s IP in the browser, appending :8081 to test the new configuration.

## 8. **Create an SSH Alias**

1. Add an alias for easier login:
```bash
alias vserver_connect="ssh -o StrictHostKeyChecking=False -i ~/.ssh/id_ed25519 username@Server-IP"
```
2. Use the alias to log in:
```bash
vserver_connect
```
## 9. **Install and Configure Git**

1. Install Git:
```bash
sudo apt update
sudo apt install git -y
```
2. Configure Git:
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
3. Verify Git installation:
```bash
git --version
```
4. Test Git functionality by cloning a repository:
```bash
git clone https://github.com/example/repository.git
```
5. Navigate into the cloned directory:
```bash
cd repository
```

### You have successfully set up your V-Server and configured it for secure access and web hosting!