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
  I created new files for each of my Environment in my inventory folder and named them dev.yml, staging.yml, uat.yml and prod.yml
  Also created a new file in my playbooks folder and named it common.yml
  
  ![create new files in inventory](https://user-images.githubusercontent.com/79808404/191039904-4e7b4a55-e04e-433c-aea4-ffc8e20977d6.JPG)

 ## Settling up Ansible Inventory
 
 ### Step 1
   I installed OpenSSH using PowerShell and ran the Powershell as an administrator and input the below command 
    
      Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
     
   and used the below command to Instaled  the _Not Present OpenSSH Server _
   
       Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
         
  ![Add ssh powershell](https://user-images.githubusercontent.com/79808404/192166975-0e6d0d94-7e56-4d29-a403-7d8983c6e7f9.JPG)

  I confirmed if the OpenSSH server is installed as expected with the command 
   
        Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
        
   ![installed ssh](https://user-images.githubusercontent.com/79808404/192167286-9c634e54-f140-4c2a-a45d-0ea2dc76a8c1.JPG)

 
 ### Step 2
   I used the below command to start and configured OPenSSH Server for intial use.
   
       # Start the sshd service
         Start-Service sshd

       # OPTIONAL but recommended:
         Set-Service -Name sshd -StartupType 'Automatic'

       # Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
       if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
    } else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
    }
 
 ![start ssh service](https://user-images.githubusercontent.com/79808404/192167311-a50c8db5-b37d-43da-b37c-d744aa827589.JPG)

  ### Step 3
  I generated key files using the Ed25519 algorithm by running the below command from my powershell
      
      ssh-keygen -t ed25519
      
 ![ssh keygen](https://user-images.githubusercontent.com/79808404/192167776-cd739546-0e25-4e63-8a7d-4bf6659abf34.JPG)

      
   ### Step 4
   I ran the below command to start my SSH command each time my system is rebooted 
       
     # This starts the ssh-agent automatically
        Get-Service ssh-agent | Set-Service -StartupType Automatic  
       
     # Start the service
        Start-Service ssh-agent

     # This should return a status of Running
       Get-Service ssh-agent
       
   ![ssh agent running](https://user-images.githubusercontent.com/79808404/192168014-b4c7cc31-c11d-4c47-a742-10b335a49518.JPG)

   and ran the command below to add my private key to my ssh agent
   
     ssh-add Projects.pem
  
  ![ssh add projectPem](https://user-images.githubusercontent.com/79808404/192168203-0610c744-ce3d-419d-bb27-a0be8d80ee73.JPG)
  
  ### Step 5
   I confirmed my private key has been added by running the command below
     
       ssh-add -l
   
  ![ssh add -l](https://user-images.githubusercontent.com/79808404/192168535-99faacd8-ad8e-413f-989c-8043c2f09cdc.JPG)
   
 ###  Step 6
   
   I ssh into my jenkins-ansible server using ssh-agent
   
     ssh -A ubuntu@(my jenkin-ansible public-ip)
  
 ![connect to ssh instance 2tru IP](https://user-images.githubusercontent.com/79808404/192168716-e3050d3b-d16b-4af5-af9c-7caac4aa2336.JPG)
 
 
 ### Step 7
  
   I launched 4 new Redhat 8 instances ( NFS, DB, Webserver-1, Webserver-2) and an Ubuntu 20.01 instance (Load Balancer)
   
   ![redhat instance](https://user-images.githubusercontent.com/79808404/193419847-f93a5016-035e-404f-990b-a6d3574c3694.JPG)

![LB](https://user-images.githubusercontent.com/79808404/193419852-1a5ce7b5-8d56-451a-be86-d78175928ef9.JPG)

![instance2](https://user-images.githubusercontent.com/79808404/193419854-2c373676-1ba4-4f33-9b94-f9df704a83ac.JPG)

### Step 8
  I ssh into my NFS server using my NFS Public IP to confirm that my ssh agent works as expected
  
  ![nfs ssh](https://user-images.githubusercontent.com/79808404/193419951-789a7c77-1695-4e79-9e31-41e3ef0fb7e5.JPG)

  
## CREATE A COMMON PLAYBOOK AND UPDATE INVENTORY/Dev.yml

### Step 1
  I updated my inventory/dev.yml file with the below code
    
      [nfs]
      <NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

      [webservers]
      <Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
      <Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

      [db]
      <Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

      [lb]
      <Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
    
   ![devYml](https://user-images.githubusercontent.com/79808404/193420810-7258f7e2-cece-40b9-b4b9-c72e2ad7347a.JPG)


### Step 2
  I updated my playbooks/common.yml file with the code below
  
    ---
    - name: update web, nfs and db servers
      hosts: webservers, nfs, db
      remote_user: ec2-user
      become: yes
      become_user: root
      tasks:
        - name: ensure wireshark is at the latest version
          yum:
            name: wireshark
            state: latest

    - name: update LB server
      hosts: lb
      remote_user: ubuntu
      become: yes
      become_user: root
      tasks:
        - name: Update apt repo
          apt: 
            update_cache: yes

        - name: ensure wireshark is at the latest version
          apt:
            name: wireshark
            state: latest
            
  
   ![commonYML](https://user-images.githubusercontent.com/79808404/193420922-f5e0ad33-c8e8-4016-9aec-59fff5afced6.JPG)


### Step 3
  I update my Git to commit and push my directories and files to my GitHub by using the command below
     
       git status

       git add .
       
       git commit -m 'updated files'
       
  ![git status](https://user-images.githubusercontent.com/79808404/193422012-572eec19-4893-4d36-a5b2-723d1539d529.JPG)

![git commit](https://user-images.githubusercontent.com/79808404/193422017-e3c8d1c7-566e-47e1-b915-41987151e06f.JPG)

 I ran the command _git status_ to check if all my files commited successfully
   
  ![notin to commit](https://user-images.githubusercontent.com/79808404/193422276-575518c2-2282-40c6-b8f5-0874c2205623.JPG)
  
  
  ### Step 4
    
   I pushed my commited files to my GitHub with the below command
      
       git push origin prj-11
       
   ![git push](https://user-images.githubusercontent.com/79808404/193424585-2c7b4d89-a2cc-4ab1-9331-c6c92221d320.JPG)
   
  ### Step 5
  
   I confirmed my files were successfully pushed to my Github account 
   
   ![pushed to github](https://user-images.githubusercontent.com/79808404/193425159-da6f021a-9a34-4951-8b73-e1a2ab34435f.JPG)

and I created a pull request from my Github account and merged pull request

   ![open a pull request](https://user-images.githubusercontent.com/79808404/193425374-0b4d40a9-cf38-4c0e-b117-1ecd8edc2074.JPG)

   
   ![merge pull request](https://user-images.githubusercontent.com/79808404/193425384-88931495-5022-45b4-a105-d7c3148acd7c.JPG)

   
   ![pull request merged](https://user-images.githubusercontent.com/79808404/193425396-be8698df-7ce6-4162-a854-63e1ed676aa2.JPG)

![pull request confirmed](https://user-images.githubusercontent.com/79808404/193426126-5a5a92ac-313d-4509-97fb-be2721d40221.JPG)


### Step 6
 
   I confirmed my ansible was able to communicate with my Github by verifying that my ansible build was updated as expected.
   
   ![confirmed ansible updated request](https://user-images.githubusercontent.com/79808404/193426238-a045310f-bb54-4d6d-942b-e95c1e31ce1b.JPG)

    
### Step 7

  I used the below code to checkout my Git to the main branch
   
      git checkout main
      
![git checkout](https://user-images.githubusercontent.com/79808404/193426752-4ad9f4c2-dac0-4bd7-8108-29903b76c5f8.JPG)



### Step 8
   I confirmed that my build artifacts was saved as expected in my Jenkins-ansible server with the command below
  
            /var/lib/jenkins/jobs/ansible/builds/8/archive/
            
  ![sudo ls var lib](https://user-images.githubusercontent.com/79808404/194777425-4c7944f1-da90-48be-8721-e9e771a20e79.JPG)
  
  ![check ansible dir](https://user-images.githubusercontent.com/79808404/196000716-8c9e2184-8c17-4a32-a0bb-a4e5e0ff3e17.JPG)



## _Run Ansible Test_

### Step 9
  I updated my ssh config file with the script below
  
         Host jenkins-ansible
            HostName 13.41.104.110
            User ubuntu
            IdentityFile /Users/Dell/Downloads/Projects.pem
            ForwardAgent yes
            ControlPath /tmp/ansible-ssh-%h-%p-%r
            ControlMaster auto
            ControlPersist 10m
            
   ![Screenshot (9)](https://user-images.githubusercontent.com/79808404/195994348-75847b2d-68bf-409e-9866-595df5d6ee5e.png)
    
   ![config](https://user-images.githubusercontent.com/79808404/195994628-0541f34c-f3d0-4471-84e7-d71ea52f8165.JPG)       
    

### Step 10
   I conected to my remote server from my jenkins-ansible terminal and _conected to host_ by ssh using my jenkins public Ip
     
        ssh ubuntu@ <my jenkins-ansible public Ip>
          
![jenkin connect to host](https://user-images.githubusercontent.com/79808404/195993138-62473416-0f83-4f57-9bc1-088462ead8a2.JPG)

### Step 11
  In my ssh: jenkins-ansible remote server I opened my ubuntu folder
  
   ![open folder ubuntu](https://user-images.githubusercontent.com/79808404/195994999-321e8dd6-cce5-4ac3-ac94-96563f4d9dc9.JPG)


### Step 12
   I ran my ansible-playbook using the command below
   
      ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build number>/archive/playbooks/common.yml
     
        
   ![run ansible-playbook](https://user-images.githubusercontent.com/79808404/195995571-d71e6336-131b-4851-a322-142b3ce9d224.JPG)
   
### Step 13
  
   I confirmed wireshark is installed on my servers as instructed by ssh into each server using private ip address
     
        ssh ec2-user@<nfs-private-ip>
        
        ssh ec2-user@<webserver1-private-ip>
        
        ssh ec2-user@<webserver2-private-ip>
        
        ssh ec2-user@<db-private-ip>
        
        ssh ubuntu@<LB-private-ip>
        
   ![webserver 1 confirmd](https://user-images.githubusercontent.com/79808404/195996290-8e1777ad-119b-43db-8a48-c4661e3271d0.JPG)
   
   
   ![webserver 2 confirmd](https://user-images.githubusercontent.com/79808404/195996297-ac527a1c-f5c0-4ee7-a971-8c438d8c6fba.JPG)


![db server confirmed](https://user-images.githubusercontent.com/79808404/195996308-f9883eed-e798-4c33-b134-ce23cd8ed001.JPG)


![nfs server confirmd](https://user-images.githubusercontent.com/79808404/195996351-f08e82c4-0e74-4e29-936e-748e53106850.JPG)



   
 ##  **I updated my ansible playbook with a new ansible task and go through the whole cycle again**
  
### Step 14     
     
   I checkout from my Git main to Git prj-11 and added the below script in my ansible common.yml file, commit and push the changes to my GitJub account.
     
     
     - name: create directory, file and set timezone on all servers
       hosts: webserver, nfs, db, lb
       become: yes
       tasks: 

          - name: create a directory
            file: 
              path: /home/sample
              state: directory

          - name: create a file
            file:
              path: /home/sample/ansible.txt
              state: touch

          - name: set timezone
            timezone:
              name: Africa/Lagos
   

   ![git add more task](https://user-images.githubusercontent.com/79808404/195998691-4085b3bd-0775-4a72-8b3c-0d64a2839ffe.JPG)

### Step 15
  
  I confirmed in my Github account that the file was pushed as expected and created a pull request and merged the request.
 
   ![git push confrim](https://user-images.githubusercontent.com/79808404/195998917-c3efe719-c2ce-42b8-95e8-78c933107ed3.JPG)


   ![cmpare and pull request](https://user-images.githubusercontent.com/79808404/195998923-f6f3a4ac-5044-4aa2-835c-899124369c3e.JPG)


   ![merge pull request](https://user-images.githubusercontent.com/79808404/195998930-3cc0e9af-7f08-44e3-8a57-5663a50edfbb.JPG)


![pull request merged](https://user-images.githubusercontent.com/79808404/195998946-b279d2a4-5143-4d61-8ef7-783bcf36e0e8.JPG)

   
 ### Step 16
  
   I confirmed in my jenkins account that the files was successfully pushed
   
   
   ![jenkin more tasks added](https://user-images.githubusercontent.com/79808404/195999245-f510aa2e-0b6f-4880-8839-ffbe32103982.JPG)
   
 ### Step 17
  I ran my ansible playbook with the command
  
    
  ![ansible playbook more task](https://user-images.githubusercontent.com/79808404/195999539-6757eec2-0434-41f0-9e66-66ae89e153e9.JPG)


### Step 18
  I confirmed that the task was carried out as expected  in my servers by ssh into my servers using the private ips and confirmed that both directory and files are created.
  
      ssh ec2-user@<Nfs-private-server-IP>
      
      ssh ubuntu@<LB-private-server-IP>
      
  
    
   ![nfs server sample file](https://user-images.githubusercontent.com/79808404/195999910-c5df1d0f-0273-4e31-b518-8f071da30fb8.JPG)


![LB server sample file](https://user-images.githubusercontent.com/79808404/195999914-2225678b-0b29-42b5-8a61-e325e8b068d2.JPG)

   
   

   
   
   
   
   
   
     
   
        
   
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
     
  


  
     
     
  
  
  
























  
