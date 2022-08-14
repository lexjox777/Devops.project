 # Implementation of Continous Integration Solution Using Jenkins CI
 
 
 


### STEP 1
  I created an EC2 instance based on Ubuntu server 22.04 LTS in my AWS console and named it Jenkins with opened port 8080.
  
  ![jenkin 8080](https://user-images.githubusercontent.com/79808404/184369482-76a74520-0f42-4774-9c03-ee72d5fb1f00.JPG)


### STEP 2
  I connected my ssh key to my terminal and ran the command below to update all outdated depencies in my server
     
        sudo apt update
![sudo update](https://user-images.githubusercontent.com/79808404/184370029-1703a7b3-a8bf-4985-9231-19ff581ba5a4.JPG)

 ### STEP 3
   I installed JDK  using the command 
     
       sudo apt install default-jdk-headless
   
   ![sudo install jdk](https://user-images.githubusercontent.com/79808404/184370691-7cf66ece-fc90-4d1a-9c61-9e75571d23e3.JPG)

### STEP 4
  I installed jenkins by running the command below
    
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
    sudo apt update
    sudo apt-get install jenkins
    
  ![install jenkins](https://user-images.githubusercontent.com/79808404/184370901-8101166b-e606-4bd7-b2b1-1ed739ce819e.JPG)


and used the command below to check my jenkins status
  
    sudo systemctl status jenkins
 ![jenkins status](https://user-images.githubusercontent.com/79808404/184371720-d32a0ce4-7bef-4c02-b4f5-3c54de438767.JPG)
   
  
 ### STEP 5
 
   From my web browser i input the below url to view that my jenkins configuration works as expected
   
         http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
 
 ![unlock jenkins](https://user-images.githubusercontent.com/79808404/184372303-dabdbd71-246a-45f0-800f-49101f210c84.JPG)
      
     
  ### STEP 6
  
   I ran the command below to retrive my jenkins password from my jenkins server
    
         sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ![jenkins passwd](https://user-images.githubusercontent.com/79808404/184372821-f7778dfa-1289-470b-b89a-82e0c482fddd.JPG)


### STEP 7
  I customized my jenkins by installing suggested plugins and I created an admin user 
  
  ![install plugins](https://user-images.githubusercontent.com/79808404/184373337-69125f8f-eb91-4fae-bf4a-ccf1bb8312eb.JPG)

![install plugin jenkins](https://user-images.githubusercontent.com/79808404/184373366-c8e9c67b-86f6-4378-8901-4e1071050be6.JPG)

![jenkins ready](https://user-images.githubusercontent.com/79808404/184373459-76b23dbc-b411-4a9c-92e5-942d9ab85e87.JPG)

![welcome to jenkins](https://user-images.githubusercontent.com/79808404/184373479-5cc75980-d8bb-42f8-94c9-387ce886533b.JPG)

  
 ## Configuring Jenkins to retrieve source codes from Github using webhooks
 
 
 ### STEP 1
   I enabled webhook in my Github repository setting
   
   ![webhook](https://user-images.githubusercontent.com/79808404/184472405-ebe0112e-2979-4df5-b92c-bafcbf1031b2.JPG)

   ![webhook 3](https://user-images.githubusercontent.com/79808404/184472432-5bae58e7-a9b3-4066-b6d9-02208f9a536e.JPG)

   
  ### STEP 2
   I created a freestyle project 'tooling_github' in my Jenkins Console
        
     
 ![freestyle](https://user-images.githubusercontent.com/79808404/184473054-3b5b2ab9-e21a-48ee-9a4a-6589d372284c.JPG)
  
   
  ### STEP 3
  I connected to my Github repository by copying my repository URL from my github account and input it in my Jenkins freestyle project configuration
    
   ![repo url](https://user-images.githubusercontent.com/79808404/184473470-e393475e-41b5-42c9-8ddd-e4e8c2b97fc8.JPG)

![jenkin setup](https://user-images.githubusercontent.com/79808404/184473487-08c05171-419b-4800-a33d-f9c882e01815.JPG)


### STEP 4
  I clicked the 'Build Now' button in my Jenkins to see if my configuration works as expected
 
 ![console success](https://user-images.githubusercontent.com/79808404/184474779-47550ca6-ff32-4615-aafe-599e00b1f1aa.JPG)

  
   ### STEP 5
    
   I clicked on configure settings in my jenkins project and add the below configuration is my settings
    
       IN Build Triggers
             Github hook trigger for GITScm polling
             
        In Post Build
              Archive the artifact
              
         In Files to Archive
               **
          
   
   ![build triggers](https://user-images.githubusercontent.com/79808404/184493127-eb0f9d8c-1018-4a17-9916-5518ab6ed5e0.JPG)

### STEP 6
   
   I made some changes in my Github project repository (Readme.md file) and committed the changes , automatically the changes take effect in my Jenkins by the webhook.
   
   ![build from github](https://user-images.githubusercontent.com/79808404/184493505-ada48a0b-64fb-48d6-8e5a-9e05d4d2bbe8.JPG)


![checking jenkins](https://user-images.githubusercontent.com/79808404/184493506-845f4116-af14-4b24-a742-4aa7eb82002d.JPG)

   
  ### STEP 7
    
   I checked the artifacts stored in my Jenkins server locally by default
    
   ![trace the file in terminal](https://user-images.githubusercontent.com/79808404/184493638-4c92ebe6-2237-404c-9e98-70b0cbded884.JPG)
   
   
   ## CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH
   
 ### STEP 1
   I installed 'Publish over SSH' plugin from 'Manage Plugins' menu item in Manage Jenkins Tab
   
   ![publish over ssh](https://user-images.githubusercontent.com/79808404/184494118-b8b098d0-75cc-44f1-b8c1-57904aa8ca51.JPG)

![plugin upgrade](https://user-images.githubusercontent.com/79808404/184494137-07bd2d5d-35b5-4af8-be31-30f75050041f.JPG)

### STEP 2
  I configured my Jenkins to connect to my NFS Server from my Configure System menu and navigated to Publish Over SSH section and input the below settngs
  
     Provide jenkins server pivate key 
          
          
          ![cat projPem](https://user-images.githubusercontent.com/79808404/184495529-34fb0c5f-335c-40b6-88f9-7506d39aedc8.JPG)

          
          
     Arbitrary name
         NFS
     
     Hostname
         my NFS private IP address
              
     Username
          ec2-user
     
     Remote directory
           /mnt/apps

TCP port 22 on NFS server in my AWS EC2 console is opened
   
   ![ssh confg success](https://user-images.githubusercontent.com/79808404/184495551-51b64a5b-9f5d-476e-a3a0-58f303017d1a.JPG)

I enabled proxy compactibility in my jenkins configuration

   ![enable proxy compactibility](https://user-images.githubusercontent.com/79808404/184546721-9b5e3379-215a-45f9-8a11-d51e92fc7a6a.JPG)


I configured 'Post Build Action' in my Jenkins project with the below settings
   
     Under Build tab
        Send build artifacts over SSH
   
   ![post build action](https://user-images.githubusercontent.com/79808404/184495560-a7f1d848-f74b-46f5-a5f0-f2d6f90597b7.JPG)
   
   I updated my Readme.md files in my github repository and confirmed all my configuration works perfectly.
   
   
 ![error solved 2](https://user-images.githubusercontent.com/79808404/184546784-16bbdd80-8041-40bc-964a-811a2a3491ac.JPG)

 
 
    # CHALLENGES ENCOUNTERED
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

 
