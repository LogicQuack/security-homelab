## Exercise 2: DVWA Prep for Exploitation Practice
DVWA stands for Damn Vulnerable Web Application which is a PHP web application that is intentiall designed to be vulnerable. Thus, security proffesionals and ameteurs can test their skills in a legal, safe environement. 

### Preperation
Commands: 
```bash
sudo apt update
sudo apt install -y apache2 mariadb-server php php-mysqli php-gd libapache2-mod-php git
```

The first command searches for updates through apt, and the second ensures apache2, php (programming language that DVWA is written in), Mariadb (a database to store DVWA data), and Git (tool that helps install DVWA). 

### Installing DVWA

```bash
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
cd DVWA
sudo cp config/config.inc.php.dist config/config.inc.php
```

First, I moved into /var/www/html, the root folder for Apache. Then, I retrieved DVWA from the link with "git" and "clone". Next, I moved into the newly created software of DVWA. Finally, I copied the config file template, giving me somewhere to put in my own configurations.

### Mariadb
Commands:

```bash
sudo mariadb
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'dvwa';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

These are commands to enter the mariadb database and to configure the DVWA environment. They were all inputted one by one.

### Permissions
Commands:

```bash
sudo chown -R www-data:www-data /var/www/html/DVWA
sudo chmod -R 755 /var/www/html/DVWA
sudo chmod 666 /var/www/html/DVWA/hackable/uploads/
sudo chmod 666 /var/www/html/DVWA/config/config.inc.php
```

All four changed permissions of the designated files. Chown set ownership. And the three with 3 digit numbers are using octal notation to set permissions. 755 gives owner read, write, and execute access (r,w,x) and gives groups and other only read and execute access. 666 gives only read and write access to owner, group, and other.

### Initial Set Up
Command:

```bash
>sudo systemctl restart apache2
```

This restarts apache. From there, I used my Kali and looked up "http://192.168.122.209/DVWA/setup.php" in Firefox. I then created a database, set the DVWA security to low, and tested it with a SQL injection. I inputted "1' OR '1'='1" into a USER ID field which resulted in multiple users. This served as adequate verification. 

### Learnings
- familiarity with DVWA, MYSQL, Mariadb, and Apache
- chown command allows you to set who owns a directory or item
- setting up a database for a DVWA environment
- /var/www/html is the root directory of Apache
- octal notation, which is 3 digits, to set permissions (chmod, chown, etc.). Looks like #-#-#. The first is the owner, the second is for groups, and the third is for everyone else. 

### Disclaimer: I initially used MYSQL instead of Mariadb, but I ran into some technical difficulties. I believe it had to do with PHP. You can check Extended_Documentation.md for further clarification on what I did to troubleshoot. 
