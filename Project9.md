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

    
