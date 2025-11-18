# How-to-Install-Frappe-v16-Beta-and-ERPNext-v16-Beta-on-Ubuntu-Debian
How to Install Frappe v16 and ERPNext v16 on Ubuntu/Debian  Frappe v16 and ERPNext v16 are packed with new features and improvements. Installing them might seem complex at first, but by following this step-by-step guide, you can have a working setup in no time.


Markdown

# Frappe / ERPNext v16 Beta Installation Guide

This guide covers the installation of Frappe Bench, ERPNext v16 Beta, and HRMS on Ubuntu (specifically tailored for Ubuntu 24.04+ regarding pip policies).

## 1. System Update
You must update your system before installing dependencies.

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
2. Create User
This user will run bench and all frappe services. Never install bench under root.

Bash

sudo adduser frappe
sudo usermod -aG sudo frappe
su - frappe
cd /home/frappe
Note: If you do not want to create a new user, just replace frappe with your current username.

3. Install Core Dependencies
Bash

sudo apt-get install git -y
sudo apt-get install python3-dev -y
sudo apt-get install python3-setuptools python3-pip -y
sudo apt install python3.12-venv -y
sudo apt-get install software-properties-common -y
4. MariaDB Setup
Install Server
Bash

sudo apt install mariadb-server -y
sudo mysql_secure_installation
Secure Installation Configuration
When prompted, use the following inputs:

Enter current password for root: (Press ENTER)

Switch to unix_socket authentication? Y

Change root password? Y

Remove anonymous users? Y

Disallow root login remotely? N

Remove test database? Y

Reload privilege tables? Y

Configure MariaDB
Edit the configuration file:

Bash

sudo nano /etc/mysql/my.cnf
Paste the following content into the file:

Ini, TOML

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
Restart MariaDB to apply changes:

Bash

sudo service mysql restart
5. Install Redis
Bash

sudo apt-get install redis-server -y
6. Install Node.js (v22 LTS)
Install the latest stable Node.js and npm.

Step 1: Remove old versions (to avoid conflicts)

Bash

sudo apt remove nodejs npm -y
sudo apt autoremove -y
Step 2: Add NodeSource Repository & Install

Bash

# Update system packages
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl ca-certificates gnupg

# Add NodeSource repository for Node 22 LTS
curl -fsSL [https://deb.nodesource.com/setup_22.x](https://deb.nodesource.com/setup_22.x) | sudo -E bash -

# Install Node.js (includes npm)
sudo apt install -y nodejs
Verify Installation:

Bash

node -v
npm -v
Install Yarn / NPM specifics

Bash

sudo apt-get install npm -y
7. Install PDF Tools
Required for PDF generation.

Bash

sudo apt-get install xvfb libfontconfig wkhtmltopdf -y
8. Install Frappe Bench CLI
Note: The --break-system-packages flag is required on Ubuntu 24.04+ to allow installing pip packages globally.

Bash

# Install Bench
sudo -H pip3 install frappe-bench --break-system-packages

# Install Ansible
sudo -H pip3 install ansible --break-system-packages
9. Initialize Frappe Bench (v16 Beta)
Initialize the bench specifically for the v16 beta branch.

Bash

# For v16 beta, branch name = v16.0.0-beta.1
bench init frappe-bench --frappe-branch v16.0.0-beta.1

# Switch to the Bench directory
cd frappe-bench
10. Fix User Permissions
Ensure your user has correct execute permissions on the home directory.

Bash

sudo chmod -R o+rx /home/$USER
# (or replace $USER with your frappe user if not currently logged in as them)
11. Create New Site & Install Apps
Create Site
Bash

bench new-site site_name
Enter mysql super user: root (or your configured user)

MySQL root password: (The password you set in step 4)

Set Admin password: (Set a password for the ERPNext Administrator)

Download Apps (v16 Beta)
Bash

# ERPNext v16 beta
bench get-app --branch v16.0.0-beta.1 [https://github.com/frappe/erpnext.git](https://github.com/frappe/erpnext.git)

# HRMS v16 beta
bench get-app --branch v16.0.0-beta.1 [https://github.com/frappe/hrms.git](https://github.com/frappe/hrms.git)
Install Apps to Site
Bash

# Install ERPNext
bench --site site_name install-app erpnext

# Install HRMS
bench --site site_name install-app hrms
12. Start Server
Set the site as default and start the development server.

Bash

bench use site_name
bench start

### Would you like me to generate a "Troubleshooting" section to add to the bottom of this file for common errors (like permission denied or MariaDB connection issues)?
