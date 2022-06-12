# WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS
## LEMP Stack is an example of WEB Stack Technology, which is a set of frameworks and tools used to develop a well-functioning software product.

### LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)


STEP 1...
    I started by creating an EC2 new instance on my AWS and choosing Ubuntu version 22.04.

           I created a new key pair for my instance and click on connect to copy my generated SSH client

Platform :
    Ubuntu 22.04

Platform details:
               Linux/UNIX

STEP 2.. 
   I opened my Ubuntu terminal and ran “sudo apt update” and “sudo apt upgrade” 
to update and upgrade each out-dated dependencies and packages on my system..

STEP 3... 
   I connected to my instance by running 

    “ ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”
 
from my terminal to create my Linux server in the cloud.

-------------------------------------------------------------------------------------------------------------------

## Installing Nginx and Updating my firewall

### Nginx is an open source HTTP web server which is a widely used server software. Nginx can be used as a reverse proxy, load balancer , mail proxy and Http cache to meet the needs of different environments.
   

   STEP 1..
        I installed nginx on my Ubuntu terminal by typing
                   "sudo apt update"
                   "sudo apt install nginx"
     after the installation was successfully installed, I verified that the package is installed and running 
by running 
                   "sudo systemctl status nginx"
                   
   ![nginx installed](https://user-images.githubusercontent.com/79808404/173251529-292f0744-e499-4a59-9632-35d0a9070654.PNG)

 


  STEP 2..
     I added a new rule by opening HTTP port 80 in my EC2 by editing inbound rule in my security 
group to receive any traffic by my web server. And verified I can access it locally in my Ubuntu 
by running
                 curl http://localhost:80
      ![Http port 80 confirmation](https://user-images.githubusercontent.com/79808404/173251679-025738cf-9f25-4174-baca-1e71e1c53550.PNG)

          
STEP 3..
  I tested that my Nginx Http server responds to requests from the internet by typing
                         “http:// my public Ip address :80”       
  in my web browser and shows “Welcome to Nginx!” which is a confirmation that my server is 
working as expected.

![welcome to nginx](https://user-images.githubusercontent.com/79808404/173251563-11d195d1-f249-4561-8148-b76543733fe4.PNG)



----------------------------------------------------------------------------------------------------------------------------------------

 ## INSTALLING MYSQL
 
 ### MySQL is a relational database which is used to store and manage data for websites

STEP 1..
   I installed MySQL in my Ubuntu terminal by running
                    “Sudo apt install mysql-server”
                    
   ![mysql install](https://user-images.githubusercontent.com/79808404/173251733-3640f142-0706-4a8b-a785-d01c3f74ef03.PNG)


STEP 2..
  After installation I logged into MySQL console by running
           “ Sudo mysql” 
to connect to the MySQL server as administrative database user root.


STEP 3..
  I ran a security script that was pre-installed with MySql to remove some insecure default 
settings and set a password for the root user.

mysql> “ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'MyPassword';”
Query OK, 0 rows affected (0.02 sec)
 
And Exit from MySQL console by running “exit”

STEP 4...
    I ran interactive script to validate my Password  plugin by inputting 
      “  Sudo mysql_secure_installation”  and set a secure password for my database
 After Validation to the root user password, I ran 
      “   Sudo mysql –p”
to test if I can log in to MySql console with the password created and “exit” the console after login.
![mysql password validation](https://user-images.githubusercontent.com/79808404/173252009-136bfc77-fc5b-4640-8e48-fa8d5058b670.PNG)


-------------------------------------------------------------------------------------------------------------------------------------

##  Installing PHP

   ### PHP is a component that process code to display dynamic content to the end user.

STEP 1..
    I Installed a php module that will allow my PHP to communicate with Mysql database by
 Running : 
   Sudo apt install php-mysql

Also I installed a package that will enable Nginx to handle PHP request by running
   Sudo apt install php-fpm

To install all the packages at once run the command in the terminal
     sudo apt install php-fpm php-mysql


after the installation is completed I ran 
       php –v
to check the version installed.

![check php installed version](https://user-images.githubusercontent.com/79808404/173252431-a2667e85-3202-4402-a248-9aaa87576979.PNG)

----------------------------------------------------------------------------------------------------------------------------------------

## CONFIGURING NGINX TO USE PHP PROCESSOR

STEP 1..
   I created a root web directory for my domain name ProjectLEMP by running
    sudo mkdir /var/www/projectLEMP
    
   ![create directory projectLEMP](https://user-images.githubusercontent.com/79808404/173252205-c6b92f61-1af9-417f-b649-bd82a8176340.PNG)

then I assigned the ownership of the directory by running
sudo chown -R $USER:$USER /var/www/projectLEMP

STEP 2..
  I used “nano” to open a new configuration file Nginx site-available directory by running
  sudo nano /etc/nginx/sites-available/projectLEMP

which opens a blank file and I inserted the below configuration into it and saved the file.
     
     #/etc/nginx/sites-available/projectLEMP

    server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}	
I saved and exit the file by typing “CTRL+X” and “Enter”.

STEP 3..
  I activated my config file from Nginx site-enable directory by running
   sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/

then I carried out a test for any syntax error on my configuration by running
     sudo nginx –t
     
   ![check for confg syntax error](https://user-images.githubusercontent.com/79808404/173252238-2b24fb98-06bb-4195-af4b-d236562e21a9.PNG)


  
STEP 4..  
   I disabled default Nginx host configured to listen on port 80 by running
   sudo unlink /etc/nginx/sites-enabled/default
   then I ran  “sudo systemctl reload nginx”  to apply the changes

STEP 5..
   I created an Index.html file to test that the server block works as expected by running 
      “sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html”

Then I opened my web browser and input the URL using my IP address to see the echo command from the index.html file by typing
                       http://<Public-IP-Address>:80
  
![Hello LEMP](https://user-images.githubusercontent.com/79808404/173252280-a294b1d2-7db7-4274-ab42-49062a207126.PNG)

--------------------------------------------------------------------------------------------------------------------------------------------------
  
  ## TESTING PHP WITH NGINX
  
  
 STEP 1..
  I created a test info.php file in my document root to test and validate that my Nginx can correctly hand .php files by running
         sudo nano /var/www/projectLEMP/info.php
 
then I input the line below to show information about my server
     <?php
             phpinfo(); 

STEP 2..
   I opened my web browser and input the URL with my domain name “ProjectLEMP” to access the page
        http://`server_domain_or_IP`/info.php
![info php](https://user-images.githubusercontent.com/79808404/173252334-773c0f32-1db0-46bd-8a51-bc1e907c3143.PNG)

after confirming the web server is working as expected, I ran 
“sudo rm /var/www/my_domain/info.php”  to remove the file as it contains sensitive information about my PHP environment and Ubuntu server. 


-------------------------------------------------------------------------------------------------------------------------------------------------

## RETRIEVING DATA FROM MYSQL DATABASE WITH PHP

STEP 1..
  I connected to MySQL console using root account
      sudo mysql
 then created a new database by running 
    mysql> CREATE DATABASE my_database;

STEP 2..
    I created a new user and replace the password by running
      mysql>  CREATE USER 'user1'@'%' IDENTIFIED WITH mysql_native_password BY 'new_password';

STEP 3..
  I granted the new user a full privilege by running 
    mysql> GRANT ALL ON my_database.* TO user1@'%';
  then I exited from MySQL console by typing “exit”

STEP 4..
  I log into MySQL console using the custom user and password I created
     mysql -u user1 -p
 
STEP 5..
  I created a test table named todo_list by running the following command in MySQL console  
   mysql>   CREATE TABLE my_database.todo_list (
                 item_id INT AUTO_INCREMENT,
                 content VARCHAR(255),
                 PRIMARY KEY(item_id)
                  );

STEP 6..
   I created few content in my test table using different test values
      mysql> INSERT INTO my_database.todo_list (content) VALUES ("My first important item")
 
  mysql> INSERT INTO my_database.todo_list (content) VALUES ("My second important item")

mysql> INSERT INTO my_database.todo_list (content) VALUES ("My third important item")

  STEP 7..
     I ran   mysql>  SELECT * FROM my_database.todo_list; to confirm that my data was successfully saved in my test table.  And “exit” the console upon confirmation.
  
STEP 8..
  I created a new PHP file in my web root directory by running
                   nano /var/www/projectLEMP/todo_list.php

 and paste  the below script into my todo_list.php

        <?php
        $user = "user1";
        $password = "new_password";
        $database = "my_database";
        $table = "todo_list";

        try {
        $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
        echo "<h2>TODO</h2><ol>";
        foreach($db->query("SELECT content FROM $table") as $row) {
        echo "<li>" . $row['content'] . "</li>";
        }
        echo "</ol>";
        } catch (PDOException $e) {
        print "Error!: " . $e->getMessage() . "<br/>";
        die();
        }

 I saved the file and input   "http: //<my_domain>/todo_list.php"  on my web browser to view the content in my database to confirm that my PHP environment 
is ready and it’s interacting with MySQL server.

![mysql php final image](https://user-images.githubusercontent.com/79808404/173252491-8fc66896-f3c2-4832-bfe2-4aa971a6bc4f.PNG)

----------------------------------------------------------------------------------------------------------------------------------------------------------------
  


 ## Challenges.
 
When i was trying to access MySQL console root by running "sudo mysql" I came accross the below error;

![error 1](https://user-images.githubusercontent.com/79808404/173253116-d465850f-4954-49b7-a101-251a75950c76.PNG)


## Solution

After spending couple of minutes trying to figure out what the issue might be, i checked on Google and found a solution by running "sudo mysql -uroot -p" to access the root console of my database.
 

![solution to error 1](https://user-images.githubusercontent.com/79808404/173253247-ed6b3b0c-b05d-4bab-894f-73bf0972d3c0.PNG)



 
