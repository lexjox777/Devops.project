
# **AUXILLARY PROJECT : SHELL SCRIPTING (ONBOARDING NEW USERS INTO THE SERVER)**

 ### STEP 1
 
   I started by launching a new instance on my AWS and choosing Ubuntu version 22.04.LTS

   I created a new key pair for my instance and click on connect to copy my generated SSH client

    Platform :
     Ubuntu 22.04.LTS	

    Platform details:
     Linux/UNIX


### STEP 2

   I connected to my EC2 instance by running

    “ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>”

from my terminal to create my Linux server in the cloud.
	


### STEP 3 

   I opened my Ubuntu terminal and typed _“sudo apt update”_ and _“sudo apt upgrade”_ to update and upgrade each out-dated dependencies and packages on my system.

## **CREATE A SHELL SCRIPT THAT READS CSV FILE**

### STEP 1 
   I created a Shell folder and changed directory Into it
                   
                   mkdir Shell & cd shell
		   
![mkdir Shell   cd shell](https://user-images.githubusercontent.com/79808404/176999262-e3f69398-df14-4f12-9eca-6ff8e8bf43a6.PNG)

### STEP 2

   In my shell directory, I created the following files 
                    
                  touch  Id_rsa   id_rsa.pub  names.csv
		  
![ls shell, namesCsv file](https://user-images.githubusercontent.com/79808404/176999348-f97ccc23-d67c-454a-8bec-56b529ec23f2.PNG)

### STEP 3

  I _vim_ into the _name.csv_ file and input list of Users name to be onboarded
       
                         Vim names.csv 
                         
   ![cat namesCsv](https://user-images.githubusercontent.com/79808404/176999454-9d1fba45-91a1-4035-a00a-c9e081a51705.PNG)

	
### STEP 4

  I _vim_ into the _id_rsa_ file and input my Private Key generated
  
                            Vim id_rsa
             
  ![vim PriKey](https://user-images.githubusercontent.com/79808404/176999520-4cc62fd6-6108-4f39-b755-e327404d31ef.PNG)

### STEP 5

 I _vim_ into the _id_rsa.pub_ and input my Public Key generated
                            
                            Vim id_rsa.pub
			    
   ![vim PubKey](https://user-images.githubusercontent.com/79808404/176999525-eabefbff-a97f-45d6-bae4-096cccbcd9fb.PNG)

 
 ### STEP 6
 
  I created a group and named it developers to add all our new users to this group
                        
                        Sudo groupadd developers
			
   ![group developers](https://user-images.githubusercontent.com/79808404/176999563-1ec5542d-9468-4583-b440-0da2c4bc5863.PNG)

### STEP 7

 I created a SSH file named _onboard.ssh_ and input the script below
                
                touch onboard.ssh  &  Vim onboard.ssh

           -----------------------------------------------
    #!/bin/bash
           userfile=$(cat names.csv)
           PASSWORD=password
    # To ensure the user running this script has a sudo privilege
           if [ $(id -u) -eq 0 ]; then
    # Reading the CSV file
          for user in $userfile;
           do
             echo $user
           if id "$user" &>/dev/null
           then
             echo "User Exit"
           else
    # This will create a new User
       useradd -m -d /home/$user -s /bin/bash -g developers $user
       echo " New User Created"
       echo
    # This will create a new ssh folder in the user home folder
      su - -c "mkdir ~/.ssh" $user
      echo ".ssh directory created for new user"
      echo
    # Setting a user permission for the ssh dir
       su - -c "chmod 700 ~/ .ssh" $user
       echo "user permission for .ssh directory set"
       echo
    # This will create an authorized-key file
       su - -c "touch ~/. ssh/authorized_keys" $user
      echo "Authorized Key File Created"
      echo
    # We need to set permission for the key file
        su - -c "chmod 600 ~/.ssh/authorized_keys" $user
        echo "user permission for the Authorized Key File set"
        echo
    # We need to create and set public key for users in the server
        cp -R "/home/ubuntu/shell/id_rsa.pub" "/home/$user/.ssh/authorized_keys"
        echo "Copyied the Public Key to New User Account on the server"
        echo
        echo
        echo "USER CREATED"
    # Generate a password.
      sudo echo -e "$PASSWORD\n$PASSWORD" | sudo passwd "$user"
      sudo passwd -x 5 $user
               fi
           done
       else
       echo "Only Admin Can Onboard A User"
       fi
       
### STEP 8

  I gave permission to _onboard.ssh_ file to make it an executable file.
         
            Chmod +x onboard.ssh


### STEP 9

   I changed to root user to run my _onboard.ssh_ file by running 
                              
                         sudo su
			 
  and ran my onboard.ssh file as a root user by running
                   
              ./onboard.ssh 
	      
   to create all the users in the name.csv file 
   
![sudo su onboard](https://user-images.githubusercontent.com/79808404/176999775-1bd7ecd2-d01c-4c01-9cd9-691b21928da5.PNG)

![onboard ssh give permission   run](https://user-images.githubusercontent.com/79808404/177011434-7ffad483-3d91-49b7-bb34-72c8604cb03d.PNG)

![emily user](https://user-images.githubusercontent.com/79808404/177011322-69952f3a-5782-4dfc-8b2d-764953fbd6fa.PNG)

### STEP 10
   In my root user, I ran _ls –la /home_ to view all the onboarded users in the home directory to be sure my script worked as expected.
   
   ![ls Home](https://user-images.githubusercontent.com/79808404/176999988-c7312617-32f6-4ead-9173-30c2df6c6df9.PNG)

## USING A RANDOM ONBOARDED USER TO LOGIN INTO THE SERVER USING THE PRIVATE KEY.. 

### STEP 11

 I Exited from my shell and created a pem key file _touch aux-sh.pem_ then I _nano aux-sh.pem_ to input my Private key in the file.
 
![auxSHpem](https://user-images.githubusercontent.com/79808404/177011210-02c6f02e-9369-4d77-afa8-587c1719cde5.PNG)

### STEP 12 

  I set permission to the Pem.key by running _chmod 600 aux-sh.pem_
  
  ![chmod600aux](https://user-images.githubusercontent.com/79808404/177011241-feaecf7b-ca34-485f-a8b8-fd675fa42a6a.PNG)
  
 ### STEP 13
 
   I randomly selected a user and connect the user to the server using the private
   
    “ _ ssh -i "aux-sh.pem" John@my-Private-Ip.eu-west-2.compute.amazonaws.com_

## ERROR ENCOUNTERED.

I came accross the error below when I ran my _./onboard.ssh_ script 


![error44](https://user-images.githubusercontent.com/79808404/177011636-35a3bef1-a878-41b1-af04-ffd9e08e99fa.PNG)

## SOLUTION.

 After spending couple of hours trying to debug the error, I finaly realised the error was because didn't put a space in my code when I had a conversation with a mentor. 
  
   Before
         
         # To ensure the user running this script has a sudo privilege
           if [$(id -u) -eq 0]; then
           
   After
    
          # To ensure the user running this script has a sudo privilege
           if [ $(id -u) -eq 0 ]; then
 
![solution to aux error](https://user-images.githubusercontent.com/79808404/177011712-dc170f69-12f9-41dd-a783-214f2596b76d.PNG)



