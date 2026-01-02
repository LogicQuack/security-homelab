## Exercize 2: DVWA Prep for Exploitation Practice
DVWA stands for Damn Vulnerable Web Application which is a PHP web application that is intentiall designed to be vulnerable. Thus, security proffesionals and ameteurs can test their skills in a legal, safe environement. 

### Preperation
commands: 
>sudo apt update

>sudo apt install -y apache2 mysql-server php php-mysqli php-gd libapache2-mod-php git

The first command searches for updates through apt, and the second ensures apache2, php (programming language that DVWA is written in), MYSQL (a database to store DVWA data), and Git (tool that helps install DVWA). 

### Installing DVWA

>cd /var/www/html

>sudo git clone https://github.com/digininja/DVWA.git

>cd DVWA

First, I moved into /var/www/html, the root folder for Apache. Then, I retrieved DVWA from the link with "git" and "clone". Finally, I moved into the newly created software of DVWA. 

>sudo cp config/config.inc.php.dist config/config.inc.php

This command copied the config file template, giving us something where I can put my unique data. 

### MYSQL

> Pause, ran into some technical difficulties. They are fixed. I will update this page tomorrow on 1/2/26. 
