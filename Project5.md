# IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

  ## Client-Server refers to an architecture in which two or more computers are connected together over a network to send request (client) to the other computer and the other computer “receive and respond” to the requests (Server).

### STEP 1

   I started by launching two instances (“Mysql-server” & “Mysql-client”) on my AWS console and choosing Ubuntu version 22.04.LTS

![Ec2 instance](https://user-images.githubusercontent.com/79808404/177049773-680b0c99-5cb2-43b2-bf60-8fcb40dac31e.PNG)

   I created a new key pair for my instance and click on connect to copy my generated SSH client

    Platform :
        Ubuntu 22.04.LTS

    Platform details:
        Linux/UNIX


### STEP 2

   I connected to my mysql EC2 instance by running

      “ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”

from my terminal to create my Linux server in the cloud.



### STEP 3 

   I opened my Ubuntu terminal and typed “_sudo apt update_” and “_sudo apt upgrade_” to update and upgrade each out-dated dependencies and packages on my system.

![sudo update](https://user-images.githubusercontent.com/79808404/177049731-7a5f4069-d417-41f9-a7f4-f1ff6c2d3efa.PNG)

### STEP 4

  I installed mysql-server by running 
  
           sudo apt install mysql-server
           
![install mysql server](https://user-images.githubusercontent.com/79808404/177049741-1fbfd00d-ff1d-4eb5-94b3-bec2b33e2ee3.PNG)

### STEP 5

I enabled the mysql-server by running

                sudo systemctl enable mysql
                
![enable mysql server](https://user-images.githubusercontent.com/79808404/177049764-cf4729ca-84fa-4e3c-8d82-92c78893adc4.PNG)

### STEP 6

  I opened a new terminal and ran the command sudo apt update and installed mysql-client server by running 
  
              sudo apt install mysql-client
              
![sudo install mysql client](https://user-images.githubusercontent.com/79808404/177049884-bdbc14c5-aae0-4613-9887-22196b75330d.PNG)

### STEP 7 

 I ran a command **sudo mysql** and created a root user and granted all privileges to the user
 
    CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';


### STEP 8

  I created a database test table ‘project5_db;’ by running the script
  
          CREATE DATABASE project5_db;
   
  ### STEP 9
  
 I granted the root user privilege by running the script below
 
     mysql> GRANT ALL ON PROJECT5_DB.* TO 'REMOTE_USER'@'%' WITH GRANT OPTION;
    -------    
     mysql>     FLUSH PRIVILEGES;
     --------
     mysql>   exit
     
![mysql create database and show database](https://user-images.githubusercontent.com/79808404/177050711-8b2d04e4-a89b-40c3-b3fa-da3dc6df82a0.PNG)

### STEP 10
  
   I configured mysql server to allow connections from remote hosts and replaced the default bind address to ‘0.0.0.0’

           sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

  ![mysql IP conf](https://user-images.githubusercontent.com/79808404/177051154-f4c3765b-6500-41ca-b31e-b08dc865ee29.PNG)

### STEP 11
   
 I restarted mysql to make all changes to take effect
 
        sudo systemctl restart mysql
        
        
![sudo restart mysql](https://user-images.githubusercontent.com/79808404/177051417-e591d4ae-c5a3-408b-9614-99045db32728.PNG)


### STEP 12
  
  From mysql-client server, I connected to mysql-server database engine by running the below command
 
     sudo mysql -u remote_user –h ‘ privateip address of mysqlserver’ -p

  ![sudo mysql client connect to mysql server](https://user-images.githubusercontent.com/79808404/177052060-f8d370ef-ea37-454c-b15c-af05837b4d59.PNG)

### STEP 13

  From mysql-client server I ran the script **SHOW DATABASE** to see if my server is working as expected.

![show database from mysql client](https://user-images.githubusercontent.com/79808404/177052228-b22f208b-89e1-4534-a1f8-6a35fb4450a7.PNG)

### STEP 14

  I created a new table ‘pet’ in mysql-server and assigned values into it to test if my client-server can read the information.
   
     CREATE TABLE pet ( id varchar(100), username varchar(100), email varchar(100) )
     INSERT INTO pet ( id, username, email )
     VALUES ( 2, "Sky", "sky@pet.com");
    
   ![insert into pet](https://user-images.githubusercontent.com/79808404/177057187-0f5abaff-573c-43ee-b293-4b99cfd09fbb.PNG)

          
### STEP 15

  I connected to mysql-server from my mysql-client-server by running the below command 
      
       sudo mysql -u remote_user -h 172.31.13.232 –p
       
   ![conct servr from client and showdatabs](https://user-images.githubusercontent.com/79808404/177057221-04f2ae3a-3621-4d88-8691-618d33a6af1b.PNG)


### STEP 16 

  I ran the command **SHOW DATABASES;** and ran use **project5_db;** from the database table shown to change my database

![use pro5 db frm mysqlserver](https://user-images.githubusercontent.com/79808404/177057296-35c79af6-8651-485b-9495-3bb24036c12f.PNG)


### STEP 17

I ran the command below

          SELECT * from pet; 
          
 To view the values I assigned to the pet table in the mysql-server database from the client-database.

![show server database from client](https://user-images.githubusercontent.com/79808404/177057330-f9dabd65-7b60-4c1e-8d23-e499692da0ea.PNG)


  
