## Networking and Security - Linux Server Hardening
In this section, we will:

 Block all incoming traffic except SSH (22) and HTTP (80).

 Check which ports are currently open on the system.

 Set up SSH key-based authentication to eliminate password logins.
___

### 1️ Configure Firewall to Allow Only SSH & HTTP
By default, Linux servers may have open ports that increase security risks. We will block all traffic except SSH (22) and HTTP (80).


- Enable UFW (Uncomplicated Firewall):

```bash
sudo ufw enable
```
- Allow SSH (Port 22) and HTTP (Port 80):

```bash
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
```

- Deny all other incoming traffic:

```bash
sudo ufw default deny incoming
```

- Allow all outgoing traffic (for updates, downloads, etc.):

```bash
sudo ufw default allow outgoing
```
- Apply and check rules:

```bash
sudo ufw status verbose
```
- Expected Output:

```vbnet

Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
```
---

## 2️ Check Open Ports on the System
Open ports can pose security risks. We will check them using netstat or ss.

- Use netstat to List Open Ports
```bash
sudo netstat -tulnp
```
- Example Output:

```nginx
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1234/sshd
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      5678/nginx
```
- Only SSH (22) and HTTP (80) should be open.

```bash
sudo ss -tulnp
```
### Set Up SSH Key-Based Authentication

- On Your Local Machine, Generate an SSH Key Pair:

```bash
ssh-keygen -t rsa -b 4096
```
Example Output:

```vbnet

Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa): [Press Enter]
```


#### This creates two files:

__~/.ssh/id_rsa__ → Private key (DO NOT SHARE)

__~/.ssh/id_rsa.pub__ → Public key

___

- Copy the Public Key to the Server:

```bash
ssh-copy-id user@server_ip
```
- OR, manually add the key to the server:

```bash
cat ~/.ssh/id_rsa.pub | ssh user@server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
- Disable Password-Based SSH Logins:

```bash
sudo nano /etc/ssh/sshd_config
```
- Find and update the following lines:

```nginx
PasswordAuthentication no
PermitRootLogin no
```
- Restart SSH to Apply Changes:

```bash
sudo systemctl restart sshd
```
- Test Login Without Password:

```bash

ssh user@server_ip
```

