# How-to-Install-Frappe-v16-Beta-and-ERPNext-v16-Beta-on-Ubuntu-Debian
How to Install Frappe v16 and ERPNext v16 on Ubuntu/Debian  Frappe v16 and ERPNext v16 are packed with new features and improvements. Installing them might seem complex at first, but by following this step-by-step guide, you can have a working setup in no time.
You must update your system before installing dependencies
sudo apt-get update -y
sudo apt-get upgrade -y
Create user or use existing user
This user will run bench and all frappe services. Never install bench under root.
sudo adduser frappe
sudo usermod -aG sudo frappe
su - frappe
cd /home/frappe

If you do not want to create new user just replace frappe with your username
Install Dependencies
sudo apt-get install git -y
sudo apt-get install python3-dev -y
sudo apt-get install python3-setuptools python3-pip -y
sudo apt install python3.12-venv -y
sudo apt-get install software-properties-common -y




Install Mariadb
sudo apt install mariadb-server -y
sudo mysql_secure_installation

Note
Enter current password for root: (just press ENTER)
Switch to unix_socket authentication? Y
Change root password? Y
Remove anonymous users? Y
Disallow root login remotely? N
Remove test database? Y
Reload privilege tables? Y

Config Mariadb
sudo nano /etc/mysql/my.cnf

Paste this
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
[mysql]
default-character-set = utf8mb4

Restart MariaDB:
sudo service mysql restart
Install Redis
sudo apt-get install redis-server -y
Install the latest stable Node.js and npm on Ubuntu/Debian (or WSL). 
Step 1: Remove old versions (if any)
sudo apt remove nodejs npm -y
sudo apt autoremove -y
This ensures no conflicts with old APT packages.
Step 2: Update system packages
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl ca-certificates gnupg
Add NodeSource repository for Node 22 LTS (latest stable)
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
Install Node.js (includes npm)
sudo apt install -y nodejs
This installs both:
node (runtime)


npm (package manager)
Verify installation
node -v
npm -v
Install Yarn
sudo apt-get install npm -y
Install pdf
sudo apt-get install xvfb libfontconfig wkhtmltopdf -y

Install Frappe Bench
-H → force correct HOME


--break-system-packages → allows installing pip packages on Ubuntu 24+


sudo -H pip3 install frappe-bench --break-system-packages
sudo -H pip3 install ansible --break-system-packages


Install Bench
sudo -H pip3 install frappe-bench --break-system-packages
Install Ansible
sudo -H pip3 install ansible --break-system-packages
Initialize Frappe Bench (beta branch)
For v16 beta, branch name = v16.0.0-beta.1
bench init frappe-bench --frappe-branch v16.0.0-beta.1
Initialize Frappe Bench (beta branch)
For v16 beta, branch name = v16.0.0-beta.1
bench init frappe-bench --frappe-branch v16.0.0-beta.1
Switch to the Bench Folder
cd frappe-bench
Fix User Permissions
sudo chmod -R o+rx /home/$USER
(or replace $USER with your frappe user)
Create New Site
bench new-site yoursite_name
Enter mysql super user [root]: root or your user
MySQL root password: password for above user
Set MySQL root password when prompted, then Admin password.
ERPNext v16 beta
bench get-app --branch v16.0.0-beta.1 https://github.com/frappe/erpnext.git

HRMS v16 beta
bench get-app --branch v16.0.0-beta.1 https://github.com/frappe/hrms.git
Install Apps to Your Site
ERPNext
bench --site kobel install-app erpnext

HRMS
bench --site kobel install-app hrms

Set the current site
bench use yoursite_name


Start server (Development Mode)
bench start





