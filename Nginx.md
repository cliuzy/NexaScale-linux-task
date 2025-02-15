# Your development team has requested the installation of Nginx for a new microservice. 
### Your tasks are to install and setup
##### The Nginx service to start automatically on boot.
#### check to ensure it is running properly after installation.
#### The ability to restart it if it crashes.
___
- #### install Nginx
```bash
sudo apt update && sudo apt install nginx -y
```
- #### Enable Nginx to Start on Boot
```bash
sudo systemctl enable nginx
```
- #### Check if Nginx is enabled on startup:

```bash
systemctl is-enabled nginx
```
- #### Expected Output:

```nginx
enabled
```
___

### 2️ Verify Nginx is Running Properly
- Check Nginx Status
```bash
sudo systemctl status nginx
```
If Nginx is running, you should see:

```css
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2024-02-14 10:00:00 UTC; 10s ago
```
- Test Nginx by Accessing Localhost
```bash
curl -I http://localhost
```
- Expected Output:

```
HTTP/1.1 200 OK
```
---

### 3️ Set Up Auto-Restart for Nginx If It Crashes
- Open the Nginx service file:

```bash

sudo nano /etc/systemd/system/nginx.service
```
- Add the following lines under Service:

```ini
Restart=always
RestartSec=5s
```

- Save the file and reload systemd:

```bash
sudo systemctl daemon-reload
sudo systemctl restart nginx
```
- Verification:
Check if restart rules are applied:

```bash
systemctl show nginx | grep Restart
```
Expected Output:

```ini

Restart=always
RestartSec=5s
```