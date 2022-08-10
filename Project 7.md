# Implementation of a web solution for DevOps team using LAMP stack with remote Database and NFS servers.

Project Achitecture and Goals.
  
  ![project 7 architecture](https://user-images.githubusercontent.com/79808404/183841182-3c5b63e0-6d8b-48ab-9017-8f7afd9f3415.png)



       Infrastructure: AWS
       Webserver Linux: Red Hat Enterprise Linux 8
       Database Server: Ubuntu 22.04 + MySQL
       Storage Server: Red Hat Enterprise Linux 8 + NFS Server
       Programming Language: PHP
       Code Repository: GitHub













# DEVOPS TOOLING WEBSITE SOLUTION

### STEP 1
I spin up my AWS EC2 instance and created 4 instances in my RedHAT Linux (NFS , WEBSERVER 1, WEBSERVER 2, WEBSERVER 3) and a (DB Server)in my Ubuntu Linux server. 

![instances](https://user-images.githubusercontent.com/79808404/183240848-3c70ce92-6998-4a52-a1f0-20d88ab73b8b.JPG)

### STEP 2
 I created 3 logical volumes of 10Gib each and attached the volumes to the NFS Server.
 
  ![Attach vol](https://user-images.githubusercontent.com/79808404/183241365-48a2b043-233c-4e89-b926-2d168b83ebe4.JPG)


### STEP 3
  I connected to my NFS Server from my Local machine with my EC2 SSH key and run the command _lsblk_ to view all the block devices attached to my NFS Server
    
      
 ![lsblk](https://user-images.githubusercontent.com/79808404/183357600-e44ccda9-7e08-4a17-83f5-532f94a28312.JPG)

### STEP 4
  I used the Gdisk utility to create a  partition  xvdf, xvdg , xvdh on my disk with the command
  
      sudo gdisk /dev/xvdf
      sudo gdisk /dev/xvdg
      sudo gdisk /dev/xvdh
      
  ![gdisk for partition](https://user-images.githubusercontent.com/79808404/183358352-c631ab11-260f-4321-b0b5-ff86a1ccb811.JPG)

and ran the command _lsblk_ to view the partitions created 

  ![partition created](https://user-images.githubusercontent.com/79808404/183359005-61d6e16d-3726-4df0-a89e-4f39938cb003.JPG)


### STEP 6
  I installed the LVM packages using the command 
    
      sudo yum install lvm2 -y
      
  ![sudo install lvm packages](https://user-images.githubusercontent.com/79808404/183359531-6cba4988-486c-45a0-aa97-f1717891bd4c.JPG)

and ran the command below to check for available partition
  
       sudo lvmdiskscan
             &
           lsblk
 
 ![lvmdiskscan](https://user-images.githubusercontent.com/79808404/183359858-44a9924e-101e-4be1-a4c1-e0c389aeb9c4.JPG)
   
  ![partitiion lsblk](https://user-images.githubusercontent.com/79808404/183360878-d756dcf0-8a0c-4ff5-9294-299ce23ba7da.JPG)


### STEP 7
 I created a physical volume with the command below
   
     sudo pvcreate /dev/xvdf1
     sudo pvcreate /dev/xvdg1
     sudo pvcreate /dev/xvdh1
     
  and ran the command pvs to check the physical volumes were created as expected
   
       sudo pvs
  
  ![physical vol](https://user-images.githubusercontent.com/79808404/183424687-373b0493-35fb-4335-b39e-540be6e69e1a.JPG)
   
   ![sudo pvs](https://user-images.githubusercontent.com/79808404/183424733-1344187b-2de4-4c1e-8135-908782a434f2.JPG)

 ### STEP 8
   I used the command vgcreate to create a volume group for my partition with the command below
   
      sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
      
   and ran the command below to check the volume group was created as expected
     
      sudo vgs
      
   ![sudo vgcreate](https://user-images.githubusercontent.com/79808404/183426174-69fc735b-93c9-46f4-8385-caf260376deb.JPG)


### STEP 9
 I created 3 logical volumes apps, logs and opt with the command below
 
     sudo lvcreate -n lv-apps -L 9G webdata-vg
     sudo lvcreate -n lv-logs -L 9G webdata-vg
     sudo lvcreate -n lv-opt -L 9G webdata-vg

and ran the command below to verified that my logical volumes were created as expected
   
     sudo lvs
       &
     lsblk
![lvcreate apps,logs](https://user-images.githubusercontent.com/79808404/183427334-bff1195c-f4aa-47d9-9485-1901552e69aa.JPG)
    
 ![sudo lvs](https://user-images.githubusercontent.com/79808404/183431040-c417f548-1fa8-4780-8962-0d258359e338.JPG)
 
 ![lsblk for lvcreate](https://user-images.githubusercontent.com/79808404/183431078-0240af1b-acfe-4918-982f-0e485e47f6ab.JPG)
 
  I checked my entire setup with the command below
   
     sudo vgdisplay -v #view complete setup - VG, PV, and LV
     
  ![check complete setup](https://user-images.githubusercontent.com/79808404/183476909-6ce019d9-cd56-4d3f-8866-d8fdd979e5d9.JPG)


### STEP 10
  I formated my apps, logs and opt disk as xfs with the command below
    
      sudo mkfs -t xfs /dev/webdata-vg/lv-apps
      sudo mkfs -t xfs /dev/webdata-vg/lv-logs
      sudo mkfs -t xfs /dev/webdata-vg/lv-opt
       
  ![xfs apps,logs,opt](https://user-images.githubusercontent.com/79808404/183479774-308373e6-1afb-447e-a1f4-2f252a5df721.JPG)

### STEP 11
 
  I make a directory /mnt folder to mount my logical volumes with the command below
  
     sudo mkdir /mnt/apps
     sudo mkdir /mnt/logs
     sudo mkdir /mnt/opt
     
  ![mnt](https://user-images.githubusercontent.com/79808404/183480980-dd3c5613-dad9-46d5-931d-a8e671d0481c.JPG)

### STEP 12
  I mounted my lv-apps,logs,opt on /mnt/apps,logs,opt with the comand below
    
     sudo mount /dev/webdata-vg/lv-apps /mnt/apps
     sudo mount /dev/webdata-vg/lv-logs /mnt/logs
     sudo mount /dev/webdata-vg/lv-opt /mnt/opt
  
![mount lv vol](https://user-images.githubusercontent.com/79808404/183483952-2d5e744c-de9d-43c2-ac42-8c92d7f046c7.JPG)

and used the command _lsblk_ to view if my disk is configured as expected
![mount point](https://user-images.githubusercontent.com/79808404/183485568-9f067866-5e8b-469e-8c33-0bfdf37e6d00.JPG)

### STEP 13
 I updated my packages and dependencies with the command below
   
    sudo yum -y update
  
![sudo yum update](https://user-images.githubusercontent.com/79808404/183486041-e10a5d6d-ffc3-4226-a069-75e0290cfcfc.JPG)


### STEP 14
  I configured my NFS server by installing NFS Utilities with the below command
    
       sudo yum install nfs-utils -y

   ![sudo install nfs-utils](https://user-images.githubusercontent.com/79808404/183573745-b8b5060d-5ead-4425-96b0-e528a5401991.JPG)

  and I started, enabled and checked the status of my configured NFS Server with the command below
     
        sudo systemctl start nfs-server.service
        sudo systemctl enable nfs-server.service
        sudo systemctl status nfs-server.service
        
   ![nfs confg](https://user-images.githubusercontent.com/79808404/183574928-ec87d59d-04a7-4f7c-a61f-ba59942c4f5a.JPG)
 
 
 ### STEP 15
 
 I set permission that will allow my webserver to read, write and execute files on NFS with the command below
   
     sudo chown -R nobody: /mnt/apps
     sudo chown -R nobody: /mnt/logs
     sudo chown -R nobody: /mnt/opt
     
     
     sudo chmod -R 777 /mnt/apps
     sudo chmod -R 777 /mnt/logs
     sudo chmod -R 777 /mnt/opt
  
 
 ### STEP 16
   I restarted my NFS Server after configuration with the command below
   
      sudo systemctl restart nfs-server.service
      
![restart nfs](https://user-images.githubusercontent.com/79808404/183577682-15e441b8-7009-4df4-ab1b-2f1c9d050dec.JPG)

### STEP 17
  I used my vi editor mode to configured access for clients within the same SUbnet-CIDR 
    
      sudo vi /etc/exports
      
  ![vi export](https://user-images.githubusercontent.com/79808404/183699053-6897606e-77db-46c8-8764-6d7c8f24fc70.JPG)
      
   and input the below command
     
       /mnt/apps 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
      /mnt/logs 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)
      /mnt/opt 172.31.80.0/20(rw,sync,no_all_squash,no_root_squash)    
   
   ![vi cidr](https://user-images.githubusercontent.com/79808404/183699419-f58dc0cb-edb2-4340-9bc9-f43d767e5d47.JPG)

### STEP 18
  I used the below command to check the port NFS is running on
  and I opened the port in my security group by adding a new inbound rule with the port number.
  
     rpcinfo -p | grep nfs
     
  ![check port used by NFS](https://user-images.githubusercontent.com/79808404/183702742-95dc836e-875a-4d8e-8ec7-df34944f70df.JPG)
  
  ![edit NFS inbound rule](https://user-images.githubusercontent.com/79808404/183704432-3c116d54-9a19-478f-8e88-5973cff60087.JPG)
    


## CONFIGURING DATABASE SERVER

### STEP 1
  I connected to my Database server with my DB SSH key from my local machine and ran the command below to update any outdated dependencies in my ubuntu 
  
    sudo apt update
    
 ![sudo apt update](https://user-images.githubusercontent.com/79808404/183511173-3226686d-1011-4483-9c56-e4804ac47657.JPG)

 ### STEP 2 
   I installed mysql server on my ubuntu server
   
       sudo apt install mysql-server -y
       
 ![sudo install mysql](https://user-images.githubusercontent.com/79808404/183511221-cb9ccb85-b0b7-499f-9184-4e9b05c433fd.JPG)
 
       
  ### STEP 3
  I created a database 'tooling' and created a database user 'webaccess' and conected to my webserver 1 with the CIDR IP4
  
       create database tooling;
           &
       create user 'webaccess'@'172.31.0.0/16' identified by 'password';
 
   ![ip4 cidr](https://user-images.githubusercontent.com/79808404/183514828-9462e055-b8b7-47f7-b910-ff51f64c76a3.JPG)

![db create](https://user-images.githubusercontent.com/79808404/183514845-d64599f8-401d-4bcf-9948-b966af9f80b4.JPG)

and I granted all permission to webaccess user on tooling database from webservers CIDR subnet
  
      grant all privileges on tooling.* to 'webaccess'@'172.31.0.0/16';
         
            &
      flush privileges;  

![grant priv](https://user-images.githubusercontent.com/79808404/183517289-70e26f86-1076-46b2-9078-e0db1bd0751f.JPG)



## Prepare The Webservers

### STEP 1 
  I conected my webserver 1 with the SSH keys and install nfs-utils with the command below
    
       sudo yum install nfs-utils nfs4-acl-tools -y
      
   ![nfs utils on webserver1](https://user-images.githubusercontent.com/79808404/183707272-5f6a8f6a-8681-43cb-851b-ab85b2fc9957.JPG)


### STEP 2
   I make directory var/www and mounted it to the NFS server using the NFS private Ip address and used the command _df -h_ to verified that my NFS was mounted successfully.
      
        sudo mkdir /var/www
           &
       sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
      
![sudo mkdir var](https://user-images.githubusercontent.com/79808404/183711649-e10b80ea-742e-45c6-b9df-3af35e4bfb18.JPG)
  
![mount NFS privateIP](https://user-images.githubusercontent.com/79808404/183711689-277c0649-55be-4bbf-8608-2159c9f6df2e.JPG)

  
### STEP 3
  I used the vi editor and input the below configuration 
   
     sudo vi /etc/fstab
     
        &
     <NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0   
     
  ![sudo VI fstb](https://user-images.githubusercontent.com/79808404/183720469-7c48ca12-5bd5-4209-8e5f-f3237860929c.JPG)

![sudo vi fstab](https://user-images.githubusercontent.com/79808404/183720527-1a8177d5-45cd-4382-bd7c-f6ec06cb3ca4.JPG)


  ## INSTALLING APACHE
  
### STEP 1
  I used the command below to install Remi's reposotory, Apache and PHP on my server
     
       sudo yum install httpd -y
       
  ![apache install](https://user-images.githubusercontent.com/79808404/183721379-9ec0e15f-ecf7-48da-9ce6-bcb37d16d5a0.JPG)

        sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

![sudo dnf install fedoporaproject](https://user-images.githubusercontent.com/79808404/183725683-c3050917-1988-4483-a6f4-afe322724255.JPG)

         sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

![sudo install dnf utils](https://user-images.githubusercontent.com/79808404/183724135-53bb8b67-262c-4f7d-acc0-e6e1ca2eb83a.JPG)

         sudo dnf module reset php
         
  ![sudo dnf reset](https://user-images.githubusercontent.com/79808404/183724539-d2ec215f-28b4-413a-9878-df66c6e0ef57.JPG)
     
     
         **sudo dnf module enable php:remi-7.4**
         
  ![sudo dnf enable php](https://user-images.githubusercontent.com/79808404/183725017-1c77e64e-3d23-4358-b9e5-5b54b6da0878.JPG)
   
         
         sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
         
![sudo dnf install php](https://user-images.githubusercontent.com/79808404/183724821-8437ced1-981c-44f3-9c2f-31cfd370e717.JPG)


         sudo systemctl start php-fpm
                &
         sudo systemctl enable php-fpm
                 &
         setsebool -P httpd_execmem 1

   ![start,enable php](https://user-images.githubusercontent.com/79808404/183726092-f946609d-ebd7-4be1-beb4-19a7fffb741e.JPG)

### STEP 2
   I verified that my Apache files and directory are available on my Web server /var/www and on my NFS Server /mnt/apps by creating a touch test.txt file on my web server and I view the same file on my NFS Server
   
     
   ![verify NFS mounted](https://user-images.githubusercontent.com/79808404/183771105-528323d5-6b2b-4326-8f85-8b7b85ea8f9e.JPG)

  
 ### STEP 3
   I located my Apache log folder on the web server and mount it to NFS server by running the below command
     
    sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd


![mount Nfs on httpd](https://user-images.githubusercontent.com/79808404/183772122-40283aa2-c8fe-4167-ab46-183872c90671.JPG)


### STEP 4
  I used the vi editor to configure fstab for my httpd server and input the command below
      
       sudo vi /etc/fstab
             &
       <NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd nfs defaults 0 0
 
 ![sudo vi httpd config](https://user-images.githubusercontent.com/79808404/183773550-41691c6b-e83c-49f6-ba2e-a59102ce2aee.JPG)
      
### 5TEP 5
   I installed git repository on my Web Server and initialiazed it with the command
      
      sudo yum install git
      
         &
      git init 
      
   ![install git](https://user-images.githubusercontent.com/79808404/183854345-cd044204-4214-42c3-bcf4-a57bab5cf736.JPG)
   
 ### STEP 6
   i forked out a tooling file( jenkin's and Docker file) from git repo to my git repository
     
       git clone https://github.com/darey-io/tooling.git
  
   i ran a _ls_ command to view the cloned tooling repository on my webserver and cd into the folder
   
   ![git ls](https://user-images.githubusercontent.com/79808404/183857524-14161ddf-5c9e-43b6-8a50-10e69b810d52.JPG)

    
  I deployed the html folder in the tooling to /var/www/html
      
      ls /var/www
      
      sudo cp -R html/. /var/www/html

![copy html to var html](https://user-images.githubusercontent.com/79808404/183858318-cac571aa-0066-4437-998f-97597d3256aa.JPG)

![ls html](https://user-images.githubusercontent.com/79808404/183859030-6705cc5d-446a-4c8b-ba68-75443d5da216.JPG)

### STEP 6
  I opened port 80 in my EC2 instance to allow my webserver run on http
  
   ![http port 80](https://user-images.githubusercontent.com/79808404/183863166-49569f42-4065-4d7f-84d3-8d080c046a5e.JPG)


### STEP 7
   I disabled SELinux with the command below
      
      sudo setenforce 0
    
   ![sudo setenforce](https://user-images.githubusercontent.com/79808404/183871863-be17f3ac-976c-4e28-b730-8d9b10561d77.JPG)

      
   and made the changes permanent by by editing selinux configuration with vi editor by running
     
       sudo vi /etc/sysconfig/selinux
             
   and i set 
   
         SELinux = disabled
   ![disable selinux](https://user-images.githubusercontent.com/79808404/183872082-01565205-35f1-4fe2-9e85-5cabee60384e.JPG)
      
 ### STEP 8
   I restarted my Apache to make my configuration take effect with the command below
      
          sudo systemctl restart httpd
          
   and checked the status of my Apache to see if its active with the command
        
         sudo systemctl status httpd

![sudo status httpd](https://user-images.githubusercontent.com/79808404/183872754-023f8ba6-8480-4374-af1f-20bdc24229c8.JPG)

and view if my webserver is communicating to the NFS server by inputing my webserver public ip on my web browser

   ![php lognpage](https://user-images.githubusercontent.com/79808404/183873066-249c0c2d-8250-4796-9f7a-034264fc13f1.JPG)


