## Extended Documentation

### Section 1: VM Set up (December 2025) Rocky Linux on Macbook Air With UTM

  Initially I wanted to set up Rocky Linux on my Macbook Air. I used
version 8 with virtualbox as my manager. Yet, it did not work. The ISO was not
recognized when trying to boot the operating system, and the display
output was not working either. After various attempts at fixing, nothing
seemed to work until I switched to version 9 Rocky Linux and to UTM as
my manager. The boot worked then but not the display, so I settled for a
headless system.

#### Kali Linux on CachyOS Host Machine With Qemu

  For the next VM set up, I have more information. My host machine is
CachyOS, an Arch distribution. I wanted to install Kali through the
manager Qemu. I downloaded the recommended package from their website,
designed for my manager. I ran into issues when I clicked browse locally
for the image rather than imported disk. The former resulted in the
display not being activated and the boot not recognizing the download.
Once I deleted the VM and tried again with the imported disk option, it
worked and I was able to get into the desktop after logging in.

I ran into a subsequent problem of it not connecting to the internet. My
test was

>"ping -c 4 google.com."

I checked my NIC settings, but they were fine. I then ran 

>"sudo systemctl restart libvirtd"

on my host machine to restart the virtualization service. That didn't
fix it either. I then checked if the device even exists with

>"ip a show virbr0."

Only the loopback address was shown. In Kali, I then identified any IP
addresses with

>"ip a"

but there were none. The interface name was "eth0" While still in Kali, I then used the Network Manager Command Line
Interface (nmcli). I found the active name of the connection through it
with

>"nmcli con show."

The target is called "Wired connection 1." I forced this connection down
and back up with

>"sudo nmcli con up "Wired connection 1.\""

But the connection failed. Thus, I had to use nano to bypass the network
manager to get DHCP to send an IP address. I went to the network
configuration file with 

>"sudo nano /etc/network/interfaces."

In the file, I wrote in

>"auto eth0" and "iface eth0 inet dhcp."

Notice the same interface name as before. Afterward, I stopped the
network manager with

>"sudo systemctl stop NetworkManager,"

and I restarted the legacy network service to load the configuration I
made with 

>"sudo systemctl restart networking."

The result was an IP address in the 169.254 range which indicates
failure to receive one. I then shut down the Kali VM, and destroyed the
virtual network with

>"sudo virsh net-destroy default."

I unloaded the required kernel modules with 

>"sudo modprobe -r nf_nat_ftp nf_conntrack_ftp sudo modprobe -r bridge."

I reloaded the libvert service with 

>"sudo systemctl restart libvirtd."

Finally, I started the virtual network, rebuilding the virbr0 bridge with 

>"sudo virsh net-start default
>
>sudo virsh net-autostart default."

Yet, the internet still did not connect. So, in my host machine, I
edited a configuration file by doing

>"sudo nano /etc/libvirt/network.conf,"

and I changed the firewall backend to "iptables" instead of "nftables,"
uncommenting it. The line looked like

>"firewall_backend = "iptables"

I then restarted libvertd in order to make it acknowledge my
configurations and rebuild firewall rules around iptables with

>"sudo systemctl restart libvirtd."

This time, the internet connection finally occurred.
From there, I updated the system, 

>"sudo apt update
>
>sudo apt full-upgrade -y."

I then changed the password with 

>"passwd."

#### Ubuntu Linux with Qemu (12-15-2025)

Fortunately, in contrary to the previous installations, there were no
issues. I did add a USB device which messed up the internet briefly, but
I realized I shouldn't have done so despite my internet being possible
via a NIC USB. After that, I went through the installer GUI, set up my
account, and ran an update with

>"sudo apt update && sudo apt upgrade -y."

### Section 2: Initial Network Testing and Constructing

#### Apache and SSH (12-15-2025)

First, after getting into the freshly updated ubuntu machine, I
installed the Apache server with

>"sudo apt install apache2 -y."

I then installed ssh with

>"sudo apt install openssh-server -y"

And checked the status with 

>"sudo systemctl status ssh."

I also verified if it was listening with 

>"sudo ss -tulpn | grep :22"

Afterwards, I got my IP with 

>"ip a,"

and I used that to check port status. First though, I installed nmap
with 

>"sudo apt install nmap -y."

After that, I checked the open ports with 

>"nmap -sV 192.168.122.209/24"

  Initially, I had used the flag "--p 22" instead of "--sV," which only showed
ssh because it specified the port. "--sV" is more for services in general.
Thus, the open ports are 22 (SSH) and 80 (HTTP w/ Apache), and they are
vulnerable for my Kali to exploit.

#### Nmap Scanning 12-17-25

I ran a full port scan, an aggressive scan, and a vulnerability scan from my Kali machine at my Ubuntu Server. For the full port scan, I ran

>nmap -p- 192.168.122.209

However, before that, I mistakenly did not have my other machine on, so it failed to retrieve anything of note. And then I included the IP address with the CIDR notation which scanned more than the target it seems. I realized my folly and corrected to the above command. 

After the port scan, I then ran an aggressive scan with

>nmap -A 192.168.122.209

which resulted in OS version, version detection of kernals and the routerOS, SSH keys, the server (it being Apache2), and tracerouting of one hop. It guessed Ubuntu Linux as the OS. 

I finally ran a vulnerability scan with

>nmap --script vuln 192.168.122.209

No vulnerabilites were detected including DOM based XSS, CSRF, and stored XSS.

### Section 3: DVWA Set Up (1/1/26 at time of process | 1/2/26 for documentation)

The next task I deemed worthy was creating a vulnerable environment from my Ubuntu with Damn Vulnerable Web Application (DVWA). To do so, I needed a programming language, a web server, a database, and a means to getting the application off github. The language was PHP becuase DVWA is built with it, the web server was the familiar Apache which I put in the code to install despite already having it, the database was MYSQL, and the vector for retrieving DVWA was Git. 

#### Preperation

First I ran an update and downloaded the neccesseties with

>sudo apt update
>
>sudo apt install -y apache2 mysql-server php php-mysqli php-gd libapache2-mod-php git

I then entered Apache's root directory, retrieved DVWA with Git, and moved into a new DVWA file. This was all with these three commands:

>cd /var/www/html
>
>sudo git clone https://github.com/digininja/DVWA.git
>
>cd DVWA

Next, I copied the config file, so I could start putting in actual configurations based on the template in that copy.

>sudo cp config/config.inc.php.dist config/config.inc.php

#### Configuring MYSQL

The subsequent steps ended up with a few issues that I will get to. First, I went to configure MYSQL with

>sudo mysql

Then, in the MYSQL prompt that showed up, I put in these commands one by one:

>CREATE DATABASE dvwa;
>
>CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'dvwa';
>
>GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
>
>FLUSH PRIVILEGES;
>
>EXIT;

Afterward, I set permissions of the /var/www/html/DVWA file I was working with.

>sudo chown -R www-data:www-data /var/www/html/DVWA
>
>sudo chmod -R 755 /var/www/html/DVWA

This included making some internal folders writable with

>sudo chmod 666 /var/www/html/DVWA/hackable/uploads/
>
>sudo chmod 666 /var/www/html/DVWA/config/config.inc.php

After that, I restarted Apache using

>sudo systemctl restart apache2

#### Accessing DVWA and Troubleshooting

This is when I switched to my Kali Linux to access DVWA (being locally hosted on the Ubuntu) through firefox. I just threw this in the browser "http://192.168.122.209/DVWA/setup.php", and it worked. However, when I went to create the database, I recieved a 500 internal error. My troubleshooting began with the Apache logs using

>sudo tail -30 /var/log/apache2/error.log

This revealed a syntax error with PHP. While I could and should have just gone from there, I likely did a few things that I maybe did not need to. I first tested if PHP was working with 

>echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/test.php

I accessed "http://192.168.122.209/test.php" from my Kali to verify, and it was indeed working. I then checked permissions of the file I was working in through

>ls -la /var/www/html/DVWA/setup.php

There was no issue there either. After that, I checked if Apache and MYSQL were working with the systemctl status command:

>sudo systemctl status apache2
>
>sudo systemctl status mysql

They were both running. From there, I tried pulling from a different source that offered DVWA and resetting the database. Finally, I just deleted MYSQL and got Mariadb instead, an alternate database option. I did this with

>sudo apt remove mysql-server -y
>
>sudo apt install mariadb-server -y

Then, I just recreated the database, and I was finally able to create a DVWA environment. I logged in, and set the DVWA security to low as a prime target to attack. To verify if it was working fine, I tried a SQL injection test with inputting "1' OR '1'='1" into a USER ID field which resulted in multiple users. This was the validation I needed. 
