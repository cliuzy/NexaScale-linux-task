# HypotheticalCorp Linux Administrator Assessment

## this repo contains linux administrative tasks it dives deep into hands on practice of the use of terminal to carry out linux roles.

#### Hypothetical Use Case: Linux Administrator at HypotheticalCorp
Scenario:
You are the Linux Administrator at HypotheticalCorp, a growing tech company that provides cloud-based services to clients. Your responsibilities include managing user access, ensuring system security, monitoring system performance, and handling application deployments.

# System Monitoring & Performance Analysis.
### In this section, we will:
Identify the top resource-consuming processes and determine if they are necessary.

Check disk usage to ensure logs are not consuming too much space.

Monitor real-time system logs to detect anomalies
___
#### Identify Resource-Consuming Processes
During peak hours, high CPU or memory usage could slow down the server. We will use the **_top_** and **_ps_** commands to investigate.
```bash
top
```
![sc1](./images/images%2020-41-56-640/sc1.png)

Key things to look for in top output:

%CPU column: Shows which processes are using the most CPU.

%MEM column: Displays memory usage per process.

PID: The Process ID needed to investigate or terminate a process.

If a process is consuming high CPU investigate further.

#### use ps to view sorted list
```bash
ps
```
![sc2](./images/images%2020-41-56-640/sc2.png)
#### kill the process
```bash
sudo kill -9 <PID>
```

___

#### 2. Check Disk Usage (Investigate Log Size)
If logs are too large, they could be slowing down the server.

 Check Overall Disk Usage
 ```bash
 df -h
 ```

![sc3](./images/images%2020-41-56-640/sc3.png)
check which directories are taking up space
```bash
du -sh /var/log/*
```
![sc4](./images/images%2020-41-56-640/sc4.png)

#### Clear or Compress Large Log Files

Truncate large log files (without deleting them)

```bash
sudo truncate -s 0 /var/log/syslog
```
___

#### 3️. Monitor Real-Time System Logs


View Live Logs Using tail
```bash
sudo tail -f /var/log/syslog
```
 Identified high CPU/memory-consuming processes and managed them.

Checked disk usage and cleared excessive logs.

 Monitored system logs in real time for anomalies.

 ___


# Users and role managment task

 
### Your company recently hired five new developers who need access to the development server. Your task is to:
### Create user accounts for them and add them to the developers group.
### Ensure they have read and execute permissions for /var/www/project but cannot modify files.

### Restrict SSH access for two of them, who should only log in locally.
___
#### 1. login to the remote server

```bash
ssh remote server
```

#### 2. create user group called Devs or a name of your choice.

```bash
sudo groupadd dev
```

#### 3. create users with home directory

```bash
sudo useradd -m -s /bin/bash devuser1
sudo useradd -m -s /bin/bash devuser2
sudo useradd -m -s /bin/bash devuser3
sudo useradd -m -s /bin/bash devuser4
sudo useradd -m -s /bin/bash devuser5
```
#### 4. add password for each user
```bash
echo "devuser1:SecurePass1" | sudo chpasswd
echo "devuser2:SecurePass2" | sudo chpasswd
echo "devuser3:SecurePass3" | sudo chpasswd
echo "devuser4:SecurePass4" | sudo chpasswd
echo "devuser5:SecurePass5" | sudo chpasswd
```
#### 5. add all developers to the group
```bash
sudo usermod -aG developers devuser1
sudo usermod -aG developers devuser2
sudo usermod -aG developers devuser3
sudo usermod -aG developers devuser4
sudo usermod -aG developers devuser5
```
#### 6. verify if the users where added succesfully
```bash
cat /etc/group | grep developers
```
___

### Ensure they have read and execute permissions for /var/www/project but cannot modify files.

#### 1.  Ensure the project directory exists
```bash
sudo mkdir -p /var/www/project
```
#### 2. Change ownership: root owns the directory, but the 'developers' group can access it
```bash
sudo chown root:developers /var/www/project
```
#### 3. Set permissions: 
Owner (root) has full access (rwx)
Group (developers) has read & execute (r-x)
 Others have no access (---)
 ```bash
 sudo chmod 750 /var/www/project
 ```
 check for permisions
 ```bash
 ls -ld /var/www/project
 ```
 output should give
 ```
 drwxr-x--- 2 root developers 4096 Feb 14 10:00 /var/www/project
```
___

### Restrict SSH Access for Two Users
Developers devuser4 and devuser5 should only log in locally and not via SSH.
#### 1. Edit the SSH configuration file:

```nginx

sudo nano /etc/ssh/sshd_config
```
#### 2. Add the following lines at the end:

```nginx

DenyUsers devuser4 devuser5
```
#### 3️. Restart the SSH service for changes to take effect:

```nginx
sudo systemctl restart sshd
```
#### Verification:
Try logging in as devuser4 or devuser5 via SSH:

```bash

ssh devuser4@server_ip
```
Expected Output:

```bash
Permission denied
```
___

we created five developer accounts and added them to the developers group.

Configured /var/www/project directory to allow only read & execute access for developers.

 Restricted SSH access for devuser4 and devuser5, allowing only local logins.


 
