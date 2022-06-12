## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

### LAMP Stack is an example of WEB Stack Technology, which is a set of frameworks and tools used to develop a well-functioning software product.

•	### LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)


STEP 1
    I started by launching a new instance on my AWS and choosing Ubuntu version 22.04.

   I created a new key pair for my instance and click on connect to copy my generated SSH client

Platform :
    Ubuntu 22.04 version

Platform details:
    Linux/UNIX	


	
STEP 2 

   I opened my Ubuntu terminal and typed “sudo apt update” and “sudo apt upgrade” to update and upgrade each out-dated dependencies and packages on my system..

STEP 3 

   I connected to my instance by running 

    “ ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”

from my terminal to create my Linux server in the cloud.



---------------------------------------------------------------------------------------------------------------------------

 
## Installing Apache and Updating my firewall

   ### Apache is an open source HTTP server which is a widely used server software. Apache can be customised to meet the needs of different environments by using extensions and modules.
   

   STEP 1
           I installed apache on my Ubuntu terminal by typing
                 sudo apt install apache2
![apache install](https://user-images.githubusercontent.com/79808404/173253804-a56076c5-ff20-4100-bad5-3b1a6a9fe79a.PNG)

     
   and I verified that the package is installed and running by typing
                   sudo systemctl status apache2
 

  STEP 2
     I added a new rule by opening HTTP port 80 in my EC2 by editing inbound rule in my security group to receive any traffic by my web server. And verified I can access it locally in my Ubuntu by running
                 curl http://localhost:80
                 or
           curl http://127.0.0.1:80

![Http port80 verification](https://user-images.githubusercontent.com/79808404/173253826-5cd3e6aa-68a7-4404-8f63-95b1507244c1.PNG)

  
STEP 3
  I tested that my Apache Http server responds to requests from the internet by running

        “http:// my public ip address :80” 
             
  in my web browser and shows “apache2 ubuntu default page” which is a confirmation that my server is working correctly.

![Apache confirmation](https://user-images.githubusercontent.com/79808404/173253841-7e433323-c815-4088-84b2-f80457ae7107.PNG)



----------------------------------------------------------------------------


## INSTALLING MYSQL

### MySQL is a relational database which is used to store and manage data for websites

STEP 1

   I installed MySQL in my Ubuntu terminal by running
       “Sudo apt install mysql-server”

STEP 2
  After installation I logged into MySQL console by running
           “ Sudo mysql” 
to connect to the MySQL server as administrative database user root.


STEP 3
  I ran a security a security script that was pre-installed with MySql to remove some insecure default settings and set a password for the root user.

mysql> “ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'MyPassword';”
Query OK, 0 rows affected (0.02 sec)
 
And Exit from MySQL console by running “exit”

STEP 4
  I ran an interactive script by inputting 
      “  Sudo mysql_secure_installation”
  to validate my Password  plugin.
 After Validation to the root user password I ran 
      “   Sudo mysql –p”
to test if I can log in to MySql console.



  --------------------------------------------------------------------

## Installing PHP

   ### PHP is a component that process code to display dynamic content to the end user.

STEP 1
    I Installed a php module that will allow my PHP to communicate with Mysql database by running 
   Sudo apt install php-mysql

Also I installed a package that will enable Apache to handle PHP files by running
   Sudo apt install libapache2-mod-php

To install all the packages at once run the command in the terminal
     sudo apt install php libapache2-mod-php php-mysql


after the installation is completed I ran 
       php –v
to check the version installed.
![php version](https://user-images.githubusercontent.com/79808404/173253939-a71bbdb5-7acf-4234-9335-834163b504d8.PNG)



------------------------------------------------------------------------------------------------
 ## Creating a Virtual Host For Your Website using Apache

STEP 1
  I created a directory and named it projectlamp by running
        “Sudo mkdir /var/www/projectlamp”
	
I assigned ownership of the directory with my current system user by running
        “Sudo chown –R $USER:$USER /var/www/projectlamp”
I create a new blank file in Apache’s site-available directory by running
    “Sudo vi/etc/apache2/sites-available/projectlamp.conf”
And click “I” on my keyboard to enter insert mode and paste the command below to set a new configuration.
    <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
After installing the configuration, I hit the “esc” button and type “:wq” and “enter” to save and exit the file.
  Then I run “sudo ls /etc/apache2/sites-available” to check if the directory is configured.

STEP 2
  I enabled the new virtual host (projectlamp) by running
    “ Sudo a2ensite projectlamp” 
 And I disabled the default website that comes installed with Apache by running   “sudo a2dissite 000-default”
STEP 3
    I ran “sudo apache2ctl configtest” to check that the configuration file doesn’t contain any syntax error
And run “sudo systemctl reload apache2” to reload and make the changes to take effect.

STEP 4
  I created an index.html file to test that the virtual host works as expected
      sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
I opened my web browser to check if the website is working as expected by running “http:// my public ip address :80” 
  
------------------------------------------------------------------------------------
  ## ENABLE PHP ON THE WEBSITE

STEP 1
  I ran 
          sudo vim /etc/apache2/mods-enabled/dir.conf

to change the index order of the index.php to precedence over the index.html in the directoryindex directive.
And hit the “:wq” and “enter” key to save the changes. 


STEP 2
I reloaded the apache to the changes could take effect by running
       “ Sudo systemctl reload apache2”

STEP 3
  I created a new file named index.php inside my custom web root folder by running 
  vim /var/www/projectlamp/index.php

and I inserted  
             <?php
                 phpinfo();
in the blanked page opened and saved the file.

Then I refreshed the web browser to see that my PHP is working correctly.


![Php screengrab](https://user-images.githubusercontent.com/79808404/173246947-47243ff1-f773-420a-9efb-0a3a55439a22.PNG)

