# ANSIBLE CONFIGURATION MANAGEMENT TO AUTOMATE MY PROJECT

### Project architecture and scope
![pro11 architecture](https://user-images.githubusercontent.com/79808404/190804604-057866fa-a9ba-46b9-9e49-6d4c3095b6b5.JPG)



  ## **INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE**
 ### STEP 1 
   I configured my EC2 Instance in my AWS console and tagged it Jenkins-Ansible
   
   ![ec2 jenkins](https://user-images.githubusercontent.com/79808404/190898352-bec45cfa-4fc4-4963-bb9f-0e7f0ee1d0c1.JPG)

### STEP 2
  I created a new repository in my Github account and named it ansible-config-mgt
  
  ![github repo](https://user-images.githubusercontent.com/79808404/190898510-e1c1f4a2-76be-418d-8bb1-588abbdb2d23.JPG)
  
 ### STEP 3
  I connected my ssh to my terminal and run the command below to uodate all outdated dependencies in my server.
    
      sudo apt update
      
   ![sudo update](https://user-images.githubusercontent.com/79808404/190899292-025af8ef-f194-4ad6-8503-ed4efd10eabc.JPG)

 ### STEP 4
   I used the command below to install ansible in my server and checked the version if it was installed successfully
     
       sudo apt install Ansible
           &
        ansible --verision
        
   ![sudo install ansible](https://user-images.githubusercontent.com/79808404/190899684-58fba7b2-53d4-4890-a5c9-d90757addef4.JPG)
   
   ![ansible version](https://user-images.githubusercontent.com/79808404/190899694-29be06fb-5d22-4cfa-8967-3c00a28b5eea.JPG)

  ### STEP 5
   I created a new Freestyle project in my Jenkins and named it _ansible_
  
  ![anisble freestyle project](https://user-images.githubusercontent.com/79808404/190907754-c2fb2257-7494-4971-b0b4-bd49aa59a4b9.JPG)


  I configured my Jenkins by navigating to Source code management and pointed it to my github repository 'ansible-config-mgt'

  ![source code managemnt](https://user-images.githubusercontent.com/79808404/190909000-618e9d49-1e1d-42b9-b325-c5a6a075472e.JPG)

I navigated to build tiggers and checked the Github hook trigger field

   ![build triggers](https://user-images.githubusercontent.com/79808404/190909779-2c91f2ac-2aa4-473f-8471-185cddac7c9d.JPG)

I navigated to post-build tab and archive my artifact with **

  ![post-build](https://user-images.githubusercontent.com/79808404/190910560-6f4e4309-421c-4fe5-8da1-ec6de07d52cb.JPG)


### STEP 6
  I configured my Github Webhook settings and input my Jenkins url 
  
   ![github-webhook](https://user-images.githubusercontent.com/79808404/190910394-bcd416d1-4351-4a46-b4ca-50c252b55c2d.JPG)
    
   ![github-webhook2](https://user-images.githubusercontent.com/79808404/190910433-9602b121-40d5-49be-9270-0bf81b28ad58.JPG)

    
  ### STEP 7
   I ran a Test to check if my configuration works as expected by committing a change in my Readme.md Github account and check if my jenkins automatically detected the changes.
   
   ![jenkins test](https://user-images.githubusercontent.com/79808404/190910733-0c6bdc42-dfe0-4882-97e4-bd8ab56bedfe.JPG)


## Preparing development environment in my Visual Studio Code
 
 ### Step 1
   I installed a _remote development extension_ in my VSC  
 
 ![remote dev ext](https://user-images.githubusercontent.com/79808404/191036066-22f2c5f6-3326-4831-9980-aed33660434b.JPG)

  ### Step 2
   I created a clone repository of my github in my VSC
    
  ![clone repo](https://user-images.githubusercontent.com/79808404/191036750-22b474e3-8ffa-4efa-8181-a3ac5ce877d8.JPG)
  
  ![cloned repo in vsc](https://user-images.githubusercontent.com/79808404/191037078-aefbc143-75ba-461f-97b2-7c617654d8bc.JPG)

  
### Step 3
  I checked the github branch in my VS Code
  
 ![git branch](https://user-images.githubusercontent.com/79808404/191038130-86106763-75b2-46e4-97f1-51731603f6ce.JPG)

### Step 4
   I created a new branch and named it prj 11
  
 ![git new branch](https://user-images.githubusercontent.com/79808404/191039006-0975593a-175b-4daf-b46b-b9b5ae2f6249.JPG)

 
### Step 5
   I created new directories in my ansible-config-mgt folder and named them _playbooks_ and _inventory_

![mkdir playbook](https://user-images.githubusercontent.com/79808404/191039505-ad2e860b-4646-4f3e-aa65-59166a77cdf1.JPG)

### Step 6
  I created new files in my inventory folder and named them dev.yml, staging.yml, uat.yml and prod.yml
  Also created a new file in my playbooks folder and named it common.yml
  
  ![create new files in inventory](https://user-images.githubusercontent.com/79808404/191039904-4e7b4a55-e04e-433c-aea4-ffc8e20977d6.JPG)

 


  
