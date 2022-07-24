 PROJECT 6
  # WEB SOLUTION WITH WORDPRESS
  
   #### STEP 1

I.    I started by launching two instances (“webserver” & “database server”) on my AWS console and choosing RedHat Enterprise as my environment.

![1  ec2](https://user-images.githubusercontent.com/79808404/179339960-17a1725e-bda6-46be-b15e-2a3eba9f284e.PNG)

  
ii. I created 3 new volumes and set the size of each **volume to 10GiB** and the availability zone set to **us-east-1c** to be in the same region as my other instances.
 
 ![2 volume](https://user-images.githubusercontent.com/79808404/179339987-56694c5c-f1b5-4a09-8f69-0576d6885e58.PNG)


iii. I attached the volumes created to my _webserver_ instance

![3 attchVol](https://user-images.githubusercontent.com/79808404/179339995-657f4fa3-14b5-4103-af42-e7c436772e12.PNG)
 


iv. I created a new key pair for my instance and connect to my server with the generated SSH key.


### ENVIRONMENT  
    Platform :
          RedHat (Inferred)	
      Platform details:
          RedHat Enterprise Linux 8(HVM)


#### STEP 2

I connected to my web-server on my terminal with the SSH key generated and run _lsblk_ command to inspect if the 3 newly created block devices are attached to my server. 

![4 to chck vol attch](https://user-images.githubusercontent.com/79808404/179340309-e91317f4-ba8c-43c9-8fdf-946a31a3f914.PNG)
  
#### STEP 3 
I use the _df –h_ command to see all mounts and free space on my server

![5 mount and free space](https://user-images.githubusercontent.com/79808404/179340343-6fd29c80-e6e4-4e7f-9483-93718f810ead.PNG)


#### STEP 4

I use the command _sudo gdisk /dev/xvdf_  to create a partition for each of my blocks devices xvdf, xvdg and xvdh and set the code to **8E00.**  
           
           sudo gdisk /dev/xvdf
           sudo gdisk /dev/xvdg
           sudo gdisk /dev/xvdh

![8 partition3](https://user-images.githubusercontent.com/79808404/179340384-27f55721-eceb-4fbf-bed3-1c9fbf21e056.PNG)


#### STEP 5

I use the command key _lsblk_ to view the newly configured partition on each of disks created.

![9 view conf partition](https://user-images.githubusercontent.com/79808404/179340946-223d1e52-6437-4008-84e2-9cdf09d046d5.PNG)

#### STEP 6

  I installed my physical volume by running the command _sudo yum install lvm2_ 
![10 phys vol install](https://user-images.githubusercontent.com/79808404/179340957-8677c296-0360-4843-9b3d-38fd93056840.PNG)

and checked available partitions by running the command **sudo lvmdiskscan.**

![11 scan partition](https://user-images.githubusercontent.com/79808404/179341153-28afac0a-6b7c-4471-93c9-e140fbe09fa4.PNG)


#### STEP 7

  I marked my 3 newly created disk as physical volume by running the command 

            sudo pvcreate /dev/xvdf1
            sudo pvcreate /dev/xvdg1
            sudo pvcreate /dev/xvdh1
                      
  ![12  pvcreate](https://user-images.githubusercontent.com/79808404/179341184-a40aa276-c21f-4eee-aba1-e183744989b5.PNG)
         
and verified the physical volume created was successful by running the command _sudo pvs_

![13 pvs to check the phys vol created](https://user-images.githubusercontent.com/79808404/179341221-9ef4715b-4ea8-40c5-8733-cfcc985ec315.PNG)


#### STEP 8

  I used vgcreate to add all 3 physical volume (PV) to a volume group and named the Volume group (VG) webdata-vg

          sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
    
  ![14 vgcreate](https://user-images.githubusercontent.com/79808404/179341292-593a1bb8-fed1-46d5-a94a-cea419a67b12.PNG)

    
I verified that the volume group is created successfully by running the command 
    
            sudo vgs

#### STEP 9

  I used lvcreate to create 2 logical volumes app-lv (to be used to store data for the website) and logs-lv (to be used to store data for the logs) and set the PV sizes to 14gib each.
    
         sudo lvcreate -n apps-lv -L 14G webdata-vg
         sudo lvcreate -n logs-lv -L 14G webdata-vg
         
   ![15 lvcreate -n](https://user-images.githubusercontent.com/79808404/179341383-35f0dcff-c496-441c-bace-3c44bb4dd9f3.PNG)

     
I verified that my logical vol was successfully created by running the command sudo lvs

#### STEP 10

  I used mkfs.ext4 to format the logical volumes with ext4 file system
  
  ![16 mkfs ext4](https://user-images.githubusercontent.com/79808404/179341433-1fd821c7-968a-471e-a525-b47d3a384e0b.PNG)


#### STEP 11

 I created 2 directories /var/www/html ( to store website files) and /home/recovery/logs ( to store backup of log data)
 
 ![17 mkfs ext4](https://user-images.githubusercontent.com/79808404/179341567-6ec392d8-8ec7-4775-9596-540b4accd358.PNG)


 #### STEP 12
 
 I mounted  **/var/www/html** on **apps-lv** logical volume by running the command 
 
          sudo mount /dev/webdata-vg/apps-lv /var/www/html/

![18 sudo mount](https://user-images.githubusercontent.com/79808404/179341533-33b277d0-e14c-4c4d-a28e-e30feed4df01.PNG)


#### STEP 13

  I backup all the files in my log directory /var/log into /home/recovery/logs before mounting the file system by running 
  
    sudo rsync -av /var/log/. /home/recovery/logs/
  
![rsync backupfiles](https://user-images.githubusercontent.com/79808404/179342516-72b15328-2793-46c9-bee5-7144101cc4f2.PNG)

and checked the backup files are backed-up as expected by running 
  
    sudo ls -l /home/recovery/logs
    
  ![check backup on recovery](https://user-images.githubusercontent.com/79808404/179343279-65cff6c5-86ea-49e5-a95a-0b8ae548c6a3.PNG)


#### STEP 14
 
   I mounted /var/log on logs-lv logical volume by running the command below
       
          sudo mount /dev/webdata-vg/logs-lv /var/log
   
   ![12  mount var log on logs v](https://user-images.githubusercontent.com/79808404/179343018-3a6541a8-86fb-4a8a-9508-b0d3ab6c7b24.PNG)

#### STEP 15

 I restored log files back into /var/log directory by running the below command
     
     sudo rsync -av /home/recovery/logs/. /var/log


![retore log files](https://user-images.githubusercontent.com/79808404/179343893-b1409b00-b68a-4f1e-bd67-d2ab688631dd.PNG)

#### STEP 16
 
 After restoring the backed-up files back into the /var/log directory I ran the below command to be sure that the files are restored as expected
    
      sudo ls -l /var/log

![check if file restored](https://user-images.githubusercontent.com/79808404/179344076-273c5f5d-044f-46c2-81d8-0f1a44012582.PNG)



#### STEP 17
  I ran the command _sudo blkid_ to check my **block id** to populate my fstab

![sudo blkid](https://user-images.githubusercontent.com/79808404/179345219-4386e98c-0a43-4315-926d-2b9cda70499b.PNG)

I insert the _block id_ in the _fstab_ by running the command
  
    sudo vi /etc/fstab
    
![fstab insert](https://user-images.githubusercontent.com/79808404/179345316-3765240c-2602-467b-b3ad-dd9206d853eb.PNG)

I checked if my configuration works prfectly with _sudo mount -a_ command

![sudo mount -a](https://user-images.githubusercontent.com/79808404/179345359-3f198e5f-2a16-488d-b881-a47275bc1a14.PNG)

I reloaded the daemon to see if my configuration persist with the below command
   
    sudo systemctl daemon reload
  
![sudo daemon reload](https://user-images.githubusercontent.com/79808404/179345472-259db749-e870-4d51-abf8-9b4e352eed01.PNG)


I used the command _df -h_ to see if my configuration is updated 

![to be sure fstab updated](https://user-images.githubusercontent.com/79808404/179345538-67ac1210-f035-4513-9b26-96d9172e54e2.PNG)


 ## Prepare the Database Server
 
 #### STEP 1
    

  
i. I  replicated the steps above by creating 3 new volumes (db1,db2,db3) and set the size of each **volume to 10GiB** and the availability zone set to **us-east-1c** , same region as my other instances.
 

ii. I attached the volumes created to my _database-server_ instance

 

iii. I connected to my database-server on my terminal with the SSH key generated and run _lsblk_ command to inspect if the 3 newly created block devices are attached to my database-server. 

![lsblk db](https://user-images.githubusercontent.com/79808404/179419201-8b4f4b85-fa5d-4202-9fe9-87d613d4236e.PNG)

#### STEP 2
  I use the command _sudo gdisk /dev/xvdf_  to create a partition for each of my blocks devices xvdf, xvdg and xvdh and set the code to **8E00.**  
           
           sudo gdisk /dev/xvdf
           sudo gdisk /dev/xvdg
           sudo gdisk /dev/xvdh

![sudo gdisk dbase](https://user-images.githubusercontent.com/79808404/179419488-b463a550-ac8b-4e7e-a043-c12cf9edb0d8.PNG)


#### STEP 3
 
   I use the command key _lsblk_ to view the newly configured partition on each of disks created.
   ![lsblk xvd](https://user-images.githubusercontent.com/79808404/179419535-345a08ec-56f3-4c85-be76-59aa247e4173.PNG)


#### STEP 4

  I installed my physical volume by running the command _sudo yum install lvm2_ 
![10 phys vol install](https://user-images.githubusercontent.com/79808404/179340957-8677c296-0360-4843-9b3d-38fd93056840.PNG)

and checked available partitions by running the command **sudo lvmdiskscan.**


#### STEP 5

  I marked my 3 newly created disk as physical volume by running the command 

            sudo pvcreate /dev/xvdf1
            sudo pvcreate /dev/xvdg1
            sudo pvcreate /dev/xvdh1
    
  ![sudo pvcreate xvd db](https://user-images.githubusercontent.com/79808404/179419989-85e25a66-eb8c-49d3-919d-389cd204588a.PNG)
                 
         
and verified the physical volume created was successful by running the command _sudo pvs_

![pvs db](https://user-images.githubusercontent.com/79808404/179420059-ef044c72-ee13-48f7-9b08-8e676a2e5989.PNG)


#### STEP 6

  I used vgcreate to add all 3 physical volume (PV) to a volume group and named the Volume group (VG) vg-database

          sudo vgcreate vg-database /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
   
  ![vgc create db](https://user-images.githubusercontent.com/79808404/179420252-2ba47ed5-344e-49ad-ae55-c1017e1e029c.PNG)

    
I verified that the volume group is created successfully by running the command 
    
            sudo vgs
        
   ![sudo vgs db](https://user-images.githubusercontent.com/79808404/179420324-8624170d-2362-4cb0-90eb-1866e1e7a88b.PNG)
   
  
  #### Step 7
    
   I used lvcreate to create a logical volumes db-lv  and set the PV sizes to 20G.
    
         sudo lvcreate -n db-lv -L 20G vg-database
  
![sudo lvcreate db](https://user-images.githubusercontent.com/79808404/179420633-729dc149-933a-4be0-809e-b3d68c92aebc.PNG)

#### Step 8
  In the root directory I created a /db directory by running the command _sudo mkdir /db_ and created the file system by running 
       
          sudo mkfs.ext4 /dev/vg-database/db-lv
 
 ![sudo mkfs db](https://user-images.githubusercontent.com/79808404/179421023-bb4aa9ad-f6cf-4eda-a687-d7aaffd6e784.PNG)
 
  I ran the command sudo ls -l /db to be sure my directory is empty before mounting
  
  ![sudo ls -l db](https://user-images.githubusercontent.com/79808404/179421463-1a0901ce-2e19-4975-937e-7f574e70dc66.PNG)


#### STEP 9
    
   I mounted /dev/vg-database/db-lv on /db by running the command below
       
          sudo mount /dev/vg-database/db-lv /db
          
   and ran the command _df -h_ to view that my files are mounted as expected
   
   ![sudo mount db](https://user-images.githubusercontent.com/79808404/179421734-46e65067-ee97-4c5e-8948-14bb361869c2.PNG)

   
#### STEP 10
  I ran the command _sudo blkid_ to check my  **block id** to populate my fstab

![sudo blkid db](https://user-images.githubusercontent.com/79808404/179422580-9e1909f0-617c-441f-bb71-cca5f4e6a657.PNG)



I insert the _block id_ in the _fstab_ by running the command
  
    sudo vi /etc/fstab
    
 ![fstab db](https://user-images.githubusercontent.com/79808404/179422624-49d15aad-b9eb-44d7-9d6f-420ddaeea194.PNG)

    

I checked if my configuration works prfectly with _sudo mount -a_ command

![sudo mount -a db](https://user-images.githubusercontent.com/79808404/179422645-3ba9d210-33a7-4bdf-866d-63335b2e8670.PNG)



I reloaded the daemon to see if my configuration persist with the below command
   
    sudo systemctl daemon reload
  
  ![sudo reload db](https://user-images.githubusercontent.com/79808404/179422655-30b1284e-ead9-4925-8435-018017e07be7.PNG)




I used the command _df -h_ to see if my configuration is updated 


![sudo df -h db](https://user-images.githubusercontent.com/79808404/179422662-9e685f30-b3cf-465f-8f3c-3ace8cff27de.PNG)


## Installing Wordpress on my Web Server

#### STEP 1

  I started by updating my repository by running the following command 
  
      sudo yum update -y
      
  ![sudo yum update](https://user-images.githubusercontent.com/79808404/180614157-c9406a7e-d43f-4f3f-9207-991c0361850f.PNG)

      
#### STEP 2
  I installed wget, Apache and it's dependencies by the below command
  
     sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
     
 ![sudo yum install dependencies](https://user-images.githubusercontent.com/79808404/180614256-cc18d024-e4b9-4799-9108-175a97ba4704.PNG)


#### STEP 3
 I Installed PHP and its dependecies with the following command by first installing EPEL repository
 
    sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    
![install php EPEL repo](https://user-images.githubusercontent.com/79808404/180614627-97b7bb1f-2547-4ef9-b30e-bea025f81733.PNG)

#### STEP 4
I installed yum utilities and enable Remi-repository with the below command
  
    sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
   
![install yum utilities](https://user-images.githubusercontent.com/79808404/180614787-60ee83eb-a56d-45b4-81cf-0520729f3550.PNG)


#### STEP 5
  I used the below command to search for the PHP modules which are available for download.
  
    sudo yum module list php
   
 ![sudo dnf module list php](https://user-images.githubusercontent.com/79808404/180614930-0b2123ca-c534-435c-b422-dce767383074.PNG)

And I install the newer release of the PHP module with the command

    sudo dnf module reset php
 
![sudo module reset](https://user-images.githubusercontent.com/79808404/180614992-00242f3c-7af4-4a4a-abad-9f34d9690c96.PNG)


#### STEP 6

   After installing the newer version of the PHP module, I enabled it by running the command
      
    sudo dnf module enable php:remi-7.4

![sudo enable php 7 4](https://user-images.githubusercontent.com/79808404/180615115-4b238791-3198-4572-903e-0742d313cc29.PNG)


#### STEP 7
 I installed PHP , PHP-FPM (FastCGI Process Manager) and PHP Modules with the below command
   
    sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
    
 ![sudo install php](https://user-images.githubusercontent.com/79808404/180615350-82706da7-5606-46fb-8d5f-272b20e9ea11.PNG)

#### STEP 8

I verified the version of my installed PHP to be sure its working as expected

    php -v
    
 ![php version](https://user-images.githubusercontent.com/79808404/180615463-e63d1931-c5a6-4ecc-b418-d2e6d2882b9f.PNG)


#### STEP 9

I used the command below to start and enabled my PHP-FPM on boot-up
 
      $ sudo systemctl start php-fpm
      $ sudo systemctl enable php-fpm
    
![sudo systemctl start and enable php](https://user-images.githubusercontent.com/79808404/180615621-2f72f049-2930-41e5-a1a9-4102b9796773.PNG)

And checked the status of my PHP with the command 

    sudo systemctl status php-fpm

![sudo systemctl status php](https://user-images.githubusercontent.com/79808404/180615688-17445dc0-d6ed-4d2c-81e4-de730d9ce13d.PNG)

#### STEP 10
  I setup a policy by Instruct SELinux to allow Apache to execute the PHP code via PHP-FPM by running
    
     sudo setsebool -P httpd_execmem 1
 
I restarted my apache webserver for PHP to work with the Apache webserver and check the status if its running as expected by the below commands

      sudo systemctl restart httpd
      sudo systemctl status httpd

![sudo restart httpd and check status](https://user-images.githubusercontent.com/79808404/180616781-373c39a0-5b31-468b-a88c-5f61a2d64e2f.PNG)

#### STEP 11
  
I verified that my configuration is working as expected by copying my public Ip from my Aws console and ran it on my browser
  
 ![Apache redhat Test page](https://user-images.githubusercontent.com/79808404/180617577-f2c115e2-5b57-4431-b1d5-4493ce9ad8d3.PNG)


### Downloading wordpress

#### STEP 12
   I make a wordpress directory and changed into the directory
      
         mkdir wordpress
         cd   wordpress
 
![mkdir Cd wordpress](https://user-images.githubusercontent.com/79808404/180650737-1e0365bf-73d5-408b-881c-608d1281348a.PNG)

#### STEP 13
 I downloaded wordpress with the command below
   
    sudo wget http://wordpress.org/latest.tar.gz
   
  ![download wordpress](https://user-images.githubusercontent.com/79808404/180650900-5c6face6-4a2c-42cf-a25a-0ddf95a24fdc.PNG)

![ls -l to confirm wordpress](https://user-images.githubusercontent.com/79808404/180651122-ca2c5d83-8998-44d4-999d-f32c7c757f3d.PNG)


 #### STEP 14
  I extracted the file by the below command
  
     sudo tar xzvf latest.tar.gz   
     
  and changed directory to the extracted wordpress
    
     cd wordpress/
  
![cd wordpress rr](https://user-images.githubusercontent.com/79808404/180651249-de2b1923-2dfd-4a16-9635-487de8ce0091.PNG)

#### STEP 15
   I copied the files in my wp-config-sample.php into wp-config.php with the command
   
      sudo cp -R wp-config-sample.php wp-config.php

![cp config php](https://user-images.githubusercontent.com/79808404/180651439-5264385a-2e53-41f9-a959-3bcc74e7df40.PNG)

#### STEP 16
   
  I checked my present working directory and changed directory back to the wordpress folder
      
       $ pwd
        
       $ cd ..
         
![wrdpress wrdpress](https://user-images.githubusercontent.com/79808404/180651920-c58840ba-ff2b-4f42-befc-0c954ffbd797.PNG)

  
#### STEP 17
  I copied the files in my wordpress/  into var/www/html and changed directory into the var folder with the command below
  
      sudo cp -R wordpress/  /var/www/html
      
      cd /var/www/html
      
  ![cp -R wrdpress   cd var](https://user-images.githubusercontent.com/79808404/180652216-9f32731b-390d-4ef8-a8d3-7cd2e5815470.PNG)


#### STEP 18

  After copying the files from my wordpress/ into the /var/wwww/html i removed the worpress/ folder with the command below.
      
     sudo rm -rf wordpress/
 ![rm wordpress and ls](https://user-images.githubusercontent.com/79808404/180652402-3406442a-67a9-48b7-95a4-642f2dff0b04.PNG)
   
#### 19
  I changed directory back to the wordpress folder from tne var directory with the command below
  
    cd ../..
    cd wordpress
    
  ![cd back into wordpress](https://user-images.githubusercontent.com/79808404/180652647-a3e616f4-84a2-476d-9df4-e0664a0f4969.PNG)
  
 #### 20
   I used the below command to copy files from wordpress to var/wwww/html
    
       sudo cp -R wordpress/. /var/www/html/
    
   and used the command _ls-l  /var/www/html_ to verified the comtent is copied succesfully.
   
   ![cp wrdpress to var and ls var](https://user-images.githubusercontent.com/79808404/180653094-1c20c949-a7a0-4fd5-8f90-bbfa9fabc49a.PNG)

I changed my directory into var/www/html
  
    cd /var/www/html
    
 ![cd varHtml](https://user-images.githubusercontent.com/79808404/180653214-a9dc4ef0-88ea-41ed-bb83-da62de051245.PNG)

### Installing and Configuring MySQL Database to work with Wordpress

   #### STEP 1
    
 I started by updating my repository by running the command
   
      sudo yum update
   
  ![sudo yum update](https://user-images.githubusercontent.com/79808404/180657421-063cd4b9-6c56-4dfb-9a02-395fe744dfa6.PNG)
  
  I installed mysql server on both my webserver and database server with the command
    
      sudo yum install mysql-server
   
   ![sudo yum install mysql](https://user-images.githubusercontent.com/79808404/180657520-4d209f09-e4cd-4509-9f39-bb0594a85ee8.PNG)

#### STEP 2
  I enabled mysql and verified that my service is up and running  with the below command
      
         sudo systemctl restart mysqld
         sudo systemctl enable mysqld
         sudo systemctl status mysqld

![enable,start mysql](https://user-images.githubusercontent.com/79808404/180657618-71ac42bd-1328-476e-8647-779dcc5b72b2.PNG)
    
  
  #### STEP 3
     I ran the below command to access mysql console
     
        sudo mysql_secure_installation
  
  ![sudo secure installation](https://user-images.githubusercontent.com/79808404/180657880-195fc300-cd0e-4f37-b285-ddbcd462642e.PNG)
     
   I login into mysql console as a root user with the command
 
       sudo mysql -u root -p

![sudo root mysql](https://user-images.githubusercontent.com/79808404/180658066-1414b328-4815-4df9-815e-20a3888c661f.PNG)


#### STEP 4
  I created a database wordpress
     
      create database wordpress;
      
      show databases;       

![create and show database](https://user-images.githubusercontent.com/79808404/180658150-d460659a-ba75-4f86-b62a-c56e1b0b33b0.PNG)


#### STEP 5
  
    I created a user and grant the user all privileges
    
       CREATE USER 'wordpress'@'%' IDENTIFIED WITH mysql_native_password By 'Password';
       
       GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'WITH GRANT OPTION;
       
       flush privileges;

![create user mysql](https://user-images.githubusercontent.com/79808404/180658372-39b91aa8-0fb4-453c-bb16-195a86408104.PNG)

### Configure wordpress to connect to remote database

  
#### STEP 1
   from my database server I ran the command below to input my binding configuration
      
        sudo vi /etc/my.cnf
        
  ![sudo vi](https://user-images.githubusercontent.com/79808404/180661009-30ed52f3-0906-44f4-ae31-aa0961f705af.PNG)
  
  ![sudo vi binding addrss](https://user-images.githubusercontent.com/79808404/180661017-2da4119f-1efc-484d-9bb5-18627f1c6297.PNG)

  
 #### STEP 2
  I restarted my server to make my configuration to take effect with the command 
     
       sudo systemctl restart mysqld
       
 ![sudo restart mysql](https://user-images.githubusercontent.com/79808404/180661090-79759ce1-79be-4ec9-a462-88bf624ed8db.PNG)


#### STEP 3
  from my webserver I use the vi command to input my configuration 
  
      sudo vi wp-config.php
      
 ![configure wp-dtbsss](https://user-images.githubusercontent.com/79808404/180661677-3fdec92f-c86a-409c-ac8d-53ccbd69ad64.png)

 and restarted my webserver with the command
   
      sudo systemctl restart httpd
   
![sudo restart httpd](https://user-images.githubusercontent.com/79808404/180661753-bed78112-6486-4263-9f8f-e65663f4c3e6.PNG)

#### STEP 4
  In my webserver I disabled/renamed my apache test page by running the command
    
      sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup
    
  ![sudo disable apache test page](https://user-images.githubusercontent.com/79808404/180661905-82433643-11cb-4342-afff-9b7441ae239a.PNG)

  
#### STEP 6
  from my webserver i connected remotely to my database and show databases by running the below command

     sudo mysql -h myDatabaseServerPrivateIp  -u wordpress -p
     
 ![my webserver talk to my db](https://user-images.githubusercontent.com/79808404/180662055-d1b5e978-8ebc-4630-b9fc-2b0f7ff287ec.PNG)


#### STEP 7
  I change ownership to apache with the below command
  
      sudo chown -R apache:apache /var/www/html/
  
  ![chown to apache](https://user-images.githubusercontent.com/79808404/180662255-893dd4db-a8c8-4628-b723-164f323d8eae.PNG)

#### STEP 8
  I used the chcon command to change the SELinux context for file and Setsebool command to set the current state of the SELinux boolean to True with the below commands
  
    sudo chcon -t httpd_sys_rw_content_t /var/html/wordpress -R
                        &
    sudo setsebool -P httpd_can_network_connect=1
                        &
     sudo setsebool -P httpd_can_network_connect_db 1
   
 ![sudo chcon](https://user-images.githubusercontent.com/79808404/180662524-ea835668-d6a6-4c49-b00c-c53a1047446b.PNG)

 ![sudo setsebool httpd](https://user-images.githubusercontent.com/79808404/180662532-51cd9681-bf1b-4038-a39f-392bdc1c885c.PNG)

  
#### STEP 9

  I  opened my web browser and run my webserver public ip to verify if my setup works as expected.
 
  ![wordpress landing page](https://user-images.githubusercontent.com/79808404/180662669-c777a7c1-2bab-46f1-bb1f-c508a4f1247f.PNG)
  
  ![wordpress installed](https://user-images.githubusercontent.com/79808404/180662686-4d0b9cec-2efe-43a4-9b9f-25d7b22b964a.PNG)

![wordpress dashboard](https://user-images.githubusercontent.com/79808404/180662696-1a173457-b4db-4534-b9bd-812819ffb18c.PNG)

    







