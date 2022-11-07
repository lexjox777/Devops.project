#  **ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)**

 ## Project scope and architecture
  
  ![project arch](https://user-images.githubusercontent.com/79808404/197327592-9b79d643-7903-43b1-b2bf-380220030d5d.JPG)
      
         **Tasks**
          _** To use the import_playbook module as a ready solution to install or delete packages on multiple servers just with a single command.**_

## _Jenkins Job Enhancement_

 ### Step 1
   In my Jenkins server, I created a new directory _ansible-config-artifact_ and changed permission to the directory so my Jenkins could save files in there.
    
            sudo mkdir /home/ubuntu/ansible-config-artifact

                                &
            
             chmod -R 0777 /home/ubuntu/ansible-config-artifact



![mkdir artifat](https://user-images.githubusercontent.com/79808404/197327073-ecaa6f14-d88e-49f4-8c4b-cb0fca66e793.JPG)


### Step 2 
  I opened ansible-config-artifact folder in my jenkins server to monitor my ansible-config-artifacts.

![open foldr](https://user-images.githubusercontent.com/79808404/197340647-d7b013ac-1a4b-4e1e-ad7d-ab814f606450.JPG)

 ![config artifact](https://user-images.githubusercontent.com/79808404/197340655-6aa6a137-d775-4e62-b0da-f6c91270a6f3.JPG)



## Step 3
  In my Jenkins console, I navigated to the  _Manage jenkins_ tab --> _Manage plugins_ and search for _Copy Artifact_  from the Available plugins tab and Installed the plugins.
    
   ![manage plugns](https://user-images.githubusercontent.com/79808404/197339671-47051fd8-526d-43a7-ab03-ada17cc5a919.JPG)
 
   ![copy artifact](https://user-images.githubusercontent.com/79808404/197339683-2c2d83ee-cb61-4b78-97a1-8555b2fc6a3c.JPG)

   ![copy artifact installed](https://user-images.githubusercontent.com/79808404/197339692-d4a29263-471a-44b6-9822-17b1429211a4.JPG)
   
 ## Step 4
   I created and configured a new **freestyle project** in my Jenkins console and named it _save_artifacts_.
   
   ![save artifct freestyle](https://user-images.githubusercontent.com/79808404/197339855-f0ea3ece-4726-4706-8482-8f0ebd32dbe2.JPG)

   ![jenk 1](https://user-images.githubusercontent.com/79808404/197339865-3cbe083e-db9c-458b-8ed0-b98938e97c89.JPG)

   ![jenk 2](https://user-images.githubusercontent.com/79808404/197339872-57c270f8-b572-41da-b6ce-390fda837659.JPG)

   
   ![jenk 3](https://user-images.githubusercontent.com/79808404/197339877-e02b88c1-bef7-4f03-918a-1f1e5514e1c9.JPG)

   ![upstream project](https://user-images.githubusercontent.com/79808404/197340295-dbd41e25-aa7e-48a4-8ae8-d3c8520f343a.JPG)



## Step 5
  I testeed my setup by making a changes in my Readme.md file in my ansible-config-mgt repository and confirmed the files were updated in _/home/ubuntu/ansible-config-artifact_ directory
  
   ![readme update](https://user-images.githubusercontent.com/79808404/197342826-ebc21f16-0d91-4192-a42f-36152c1d39db.JPG)

  ![git push](https://user-images.githubusercontent.com/79808404/197342830-b1ac6625-6e5f-4a44-b219-14027ce00a75.JPG)

  
  ![save artifact in jenkins](https://user-images.githubusercontent.com/79808404/197342843-4adb36d2-a5e6-4321-a549-1e2b22bc970d.JPG)

   
## Step 6
  Created a new file (**site.yml**) in my playbooks directory and also created a new directory (**static-assignments**) in my ansible-config-mgt
  
   ![new file](https://user-images.githubusercontent.com/79808404/197344718-0fb5e7f4-98b2-4cc7-81ba-4260e5846bed.JPG)

   
### Step 7
  I moved common.yml file from my playbooks folder to the newly created static-assignments folder and input the below code to import playbook from static-assignment/common.yml
  
       ---
       - hosts: all
       - import_playbook: ../static-assignments/common.yml
  
  ![moved common](https://user-images.githubusercontent.com/79808404/197344931-6492e33a-1dae-4bc4-a31f-d27a76f334ed.JPG)
  
  ![host all](https://user-images.githubusercontent.com/79808404/197345218-322fb5ff-4aca-4fd1-91ee-9f17604307df.JPG)

### Step 8
  In my static-assignment folder i created a new file (common-del.yml) and input the below task to delete wireshark utilities from all servers
     
   
      ---
      - name: update web, nfs and db servers
        hosts: webservers, nfs, db
        remote_user: ec2-user
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
          yum:
            name: wireshark
            state: removed

      - name: update LB server
        hosts: lb
        remote_user: ubuntu
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
          apt:
            name: wireshark-qt
            state: absent
            autoremove: yes
            purge: yes
            autoclean: yes
   
   
   ![new task](https://user-images.githubusercontent.com/79808404/197345777-c47f822c-e8b6-4d9a-b297-3c4fef9ba099.JPG)

  
 ### I committed and pushed the update to my github and confirmed the changes took effect in my remote server as expected.
   
   
  ![new task commit](https://user-images.githubusercontent.com/79808404/197346312-dba66bba-db17-4e8e-ba0d-0c0b11d49d36.JPG)

   
  ![pushed task](https://user-images.githubusercontent.com/79808404/197346534-b8fbd3ff-9d7e-4e11-9a61-2fbd2119ece1.JPG)

   
 ### Step 9
   I updated my **site.yml** file and changed **common.yml** to **common-del.yml**  and used the below code to ping my configuration 
   
     ansible all -m ping
     
     
   ![ping ansible](https://user-images.githubusercontent.com/79808404/197422421-c176dcb7-7176-4fbb-8a59-8af1bbcadf63.JPG)
 
   
   and ran my **ansible-playbook** with the code below
     
      ansible-playbook -i /home/ubuntu/ansible-config-artifact/inventory/dev.yml /home/ubuntu/ansible-config-artifact/playbooks/site.yml
     
   ![run playbook del wireshark](https://user-images.githubusercontent.com/79808404/197422222-7997ac24-14f8-4ba2-b48c-59b640ab1fa9.JPG)


### Step 10
  I ssh into all my servers from my jenkins-ansible remote server and confirmed wireshark is deleted as expected
 
 ![wireshark del](https://user-images.githubusercontent.com/79808404/197422552-1f1c7771-83ea-487d-96d3-3e71dbc6475a.JPG)


![wireshrk del 2](https://user-images.githubusercontent.com/79808404/197422559-81a2c13b-a2b1-430a-9ffc-17dbdaa1bcdd.JPG)

### Step 11
  I created a new git branch _refactor_ 
      
      git checkout -b refactor
  
  ![git branch](https://user-images.githubusercontent.com/79808404/200199754-3e8230f2-5e6b-4972-8d78-64e8f813dad9.JPG)

      
 **## Configure UAT Webservers with a Role 'Webserver'**
 
 ### Step 1
  
  I created two new Ec2 Instances using RHEL 8 in my AWS console and named them web1-UAT and web2-UAT 
  

  I updated my ansible-config-mgt with a folder 'webserver' and created the files below in the webserver folder.
  
      └── webserver
       ├── README.md
       ├── defaults
       │   └── main.yml
       ├── handlers
       │   └── main.yml
       ├── meta
       │   └── main.yml
       ├── tasks
       │   └── main.yml
       └── templates
  
      
   ![new folders webservr](https://user-images.githubusercontent.com/79808404/200300611-896af8cc-0625-41a6-abdc-fbb4a47909c1.JPG)

 ### Step 2 
  I updated my inventory _ansible-config-mgt/inventory/uat.yml_ file with the Private Ip addresses of the web1-UAT and web2-UAT servers created.
  
    [uat-webservers]
      <Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

      <Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
     
  ![web-uat](https://user-images.githubusercontent.com/79808404/200302042-130cb777-a1fa-471b-a672-b5ed7aa5b983.JPG)


 ### Step 3
   In /etc/ansible/ansible.cfg file I uncomment _roles_path_ and provided full path to my roles directory by updating _roles_path = /home/ubuntu/ansible-config-mgt/roles_ so that ansible could know where to find configured roles.
   

     vi /etc/ansible/ansible.cfg
     
 ![vi ansible](https://user-images.githubusercontent.com/79808404/200305369-d7f966ae-7817-449c-8eb6-eb5be26c24d6.JPG)

 
 ![roles path](https://user-images.githubusercontent.com/79808404/200305292-2569fa7e-8e7c-47f6-8369-dacd45f1c62e.JPG)
 
      roles_path = /home/ubuntu/ansible-config-mgt/roles

  ![role path changed](https://user-images.githubusercontent.com/79808404/200305308-dc4737f7-5310-47cd-b462-8ff89a4772df.JPG)

### Step 4
 I updated my **Task directory** and input the below configuration tasks in the **main.yml** file
  
    ---
    - name: install apache
      become: true
      ansible.builtin.yum:
        name: "httpd"
        state: present

    - name: install git
      become: true
      ansible.builtin.yum:
        name: "git"
        state: present

    - name: clone a repo
      become: true
      ansible.builtin.git:
        repo: https://github.com/<your-name>/tooling.git
        dest: /var/www/html
        force: yes

    - name: copy html content to one level up
      become: true
      command: cp -r /var/www/html/html/ /var/www/

    - name: Start service httpd, if not started
      become: true
      ansible.builtin.service:
        name: httpd
        state: started

    - name: recursively remove /var/www/html/html/ directory
      become: true
      ansible.builtin.file:
        path: /var/www/html/html
        state: absent
  
   
  ![new task add](https://user-images.githubusercontent.com/79808404/200307692-a316600e-25e1-420e-90dc-91c00e3f18f5.JPG)

   
