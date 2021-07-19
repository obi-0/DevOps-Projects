## LAMP PROJECT

Created my AWS account and performed the following tasks:
• Signed into AWS and setup launched a new EC2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM)
• Saved my private key
• Connected to my EC2 instance through MobaXterm SSH client.
INSTALLED APACHE AND UPDATED THE FIREWALL

Install package update by running sudo apt update
Installed apache by running command `sudo apt install apache2`
Verified that apache2 is running by entering command `sudo systemctl status apache2`

![status](images/status.png)

Opened TCP port 80 which is the default port that web browsers use to access web pages on the Internet
![tcp](images/port80.png)

Verified that my web server is now correctly installed and accessible through my firewall. Opened my EC2 virtual machine public IP Address URL on my computer browser.

![apache2](images/defaultpage.png)

## INSTALLED MYSQL

Installed a Database Management System (DBMS) to be able to store and manage data for my site in a relational database. MySQL is a popular relational database management system used within PHP environments
Installed MySQL - ran this command
`sudo apt install mysql-server`

ran a security script that comes pre-installed with MySQL
`sudo mysql_secure_installation`

Validate Password plugin
After running `sudo mysql secure installation`. I was prompted to setup a password.
Tested connection to MySQL server by running `sudo mysql`

![mysql](images/mysql.png)
Exit the MySQL console by typing `mysql> exit`
