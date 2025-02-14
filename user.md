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