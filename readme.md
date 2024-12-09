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

__1. Create an `ed25519` SSH key pair using the command:__
```bash
ssh-keygen -t ed25519
```
__2. Save the keys in the .ssh directory (default location).__

__3. Verify the keys are saved with the command:__
```bash
ls ~/.ssh
```
## 2. **First Login to the Server**

__1. Connect to the server using username and Server-IP with the command:__ 
```bash
ssh username@Server-IP
```
__2. When prompted, confirm the connection by typing yes.__

__3. Enter your password to log in for the first time.__

__4. Once logged in, server information should appear.__

## 3. **Copy SSH Public Key to Server**

__1. Use Git Bash (or any terminal) on Windows to copy the SSH key:__ 
```bash
ssh-copy-id username@Server-IP
```
__2. Enter your password when prompted. This will add your public key to the server.__

## 4. **Test SSH Key Authentication**

__1. Test logging in with your SSH key:__
```bash
ssh -i ~/.ssh/id_ed25519 username@Server-IP
```
__2. You should be able to log in without entering a password.__

## 5. **Disable Password Login**

__1. Edit the SSH configuration:__
```bash
sudo nano /etc/ssh/sshd_config
```
__2. Find the line:__
```console
#PasswordAuthentication yes
```
__3. Replace it with:__
```console
PasswordAuthentication no
```
__4. Save and close the editor (Ctrl+O, then Ctrl+X).__

__5. Restart the SSH service:__
```bash
sudo systemctl restart ssh.service
```
__6. Verify password login is disabled:__
```bash
ssh -o PubkeyAuthentication=no username@Server-IP
```
The result should be: `Permission denied (publickey).`
## 6. Install Nginx Web Server

__1. Update package lists__ 
```bash
sudo apt update
```
__2. Install Nginx__ 
```bash
sudo apt install nginx -y
```
__3. Verify Nginx is running__ 
```bash
systemctl status nginx.service
```
__4. Open your Server’s IP in a Browser to check if the default Nginx page loads.__

## 7. **Configure an Alternative Nginx Site**

__1. Create a new directory for the site:__
```bash
sudo mkdir /var/www/alternatives
```
__2. Create an HTML file for the site:__
```bash
sudo touch /var/www/alternatives/alternate-index.html
```
__3. Configure the site:__
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
__4. Save and close the file (Ctrl+O, then Ctrl+X).__

__5. Edit the HTML file:__
```bash
sudo nano /var/www/alternatives/alternate-index.html
```
Customize the page as desired.

__6. Restart Nginx:__
```bash
sudo service nginx restart
```
__7. Verify the new site:__
Open your server’s IP in the browser, appending :8081 to test the new configuration.

## 8. **Create an SSH Alias**

__1. Add an alias for easier login:__
```bash
alias vserver_connect="ssh -o StrictHostKeyChecking=False -i ~/.ssh/id_ed25519 username@Server-IP"
```
__2. Use the alias to log in:__
```bash
vserver_connect
```
## 9. **Install and Configure Git**

__1. Install Git:__
```bash
sudo apt update
sudo apt install git -y
```
__2. Configure Git:__
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
__3. Verify Git installation:__
```bash
git --version
```
__4. Test Git functionality by cloning a repository:__
```bash
git clone https://github.com/example/repository.git
```
__5. Navigate into the cloned directory:__
```bash
cd repository
```

### You have successfully set up your V-Server and configured it for secure access and web hosting!