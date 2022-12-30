# Introducing Ansible Dynamic Assignments (Include) and Community Roles into my Structure
  ## (Using Ansible configuration management tool to prepare UAT environment for Tooling web solution)

### Step 1
  I created a new folder in my _ansible-config-mgt_ and named it **dynamic-assignments** .Then i created **env-vars.yml** file within the folder.
   
         ├── dynamic-assignments
         │   └── env-vars.yml
         ├── inventory
         │   └── dev
             └── stage
             └── uat
             └── prod
         └── playbooks
             └── site.yml
         └── roles (optional folder)
             └──...(optional subfolders & files)
         └── static-assignments
             └── common.yml
             
             
  ![1-dynamic folder](https://user-images.githubusercontent.com/79808404/201148551-0466b4cf-7b8c-4706-9256-112fb1ac9e96.JPG)


### Step 2
  I Created a new git branch dynamic-assignments 

![2-git branch](https://user-images.githubusercontent.com/79808404/201151477-808cb54e-d66c-4213-b938-8f647a4532ff.JPG)


### Step 3
   I created new folder **env-vars** and the below files within the env-vars folder.
      
        ├── env-vars
          └── dev.yml
          └── stage.yml
          └── uat.yml
          └── prod.yml
      
   
   ![3-new folder vars](https://user-images.githubusercontent.com/79808404/201152230-170b89c0-940b-4c74-87b3-7c446dc3b554.JPG)


### Step 4
  
  I updated the **env-vars.yml** folder with the instruction below
     
    ---
    - name: collate variables from env specific file, if it exists
      hosts: all
      tasks:
        - name: looping through list of available files
          include_vars: "{{ item }}"
          with_first_found:
            - files:
                - dev.yml
                - stage.yml
                - prod.yml
                - uat.yml
              paths:
                - "{{ playbook_dir }}/../env-vars"
          tags:
            - always
    
    
![4-new task](https://user-images.githubusercontent.com/79808404/201154280-fcbcb681-098b-4bd5-a26f-e95a7b517383.JPG)


### Step 5
   I commited all the updated changes to my Github
     
   
   ![5 - git add files](https://user-images.githubusercontent.com/79808404/201154777-d2c49fac-9b45-4648-b48f-c7fb32af778a.JPG)

### Step 6
    I updated site.yml file to make use of the dynamic assignment by inputing the below configuration
    
      ---
      - hosts: all
      - name: Include dynamic variables 
        tasks:
        import_playbook: ../static-assignments/common.yml 
        include: ../dynamic-assignments/env-vars.yml
        tags:
          - always

      -  hosts: webservers
      - name: Webserver assignment
        import_playbook: ../static-assignments/webservers.yml

![6 - update site yml](https://user-images.githubusercontent.com/79808404/201155846-ca759e3d-318d-42e3-af7a-adfd31e26717.JPG)


### Step 7
  
  I commited the changes to my Github 
    
![7 - git add update](https://user-images.githubusercontent.com/79808404/201156185-749d9a11-39e1-4837-af84-a541316141e7.JPG)

### Step 8
  I installed mysql role from ansible-galaxy community with the below command
    
     ansible-galaxy install geerlingguy.mysql
     
  and moved the file to a mysql folder within the role folder with the command below
  
      mv geerlingguy.mysql/ mysql
     
   ![1  mysql role](https://user-images.githubusercontent.com/79808404/202915076-62e85a70-d9f4-449e-b113-3dc34f4bf9ef.JPG)


![2  mysql role](https://user-images.githubusercontent.com/79808404/202915082-f11d812f-828e-47fc-8dd4-d945ed8fb8f9.JPG)



![3 mysql role created](https://user-images.githubusercontent.com/79808404/202915090-b04127eb-f336-49e6-b0f0-201f77ea6b92.JPG)

### Step 9
I uncomment and edited _mysql_database and mysql_users configuration_ setting in _main.yml_ file within the default folder of the **roles directory**.

    # Databases.
    mysql_databases: []
      - name: tooling
        collation: utf8_general_ci
        encoding: utf8
        replicate: 1

    # Users.
    mysql_users: []
     - name: webaccess
       host: 0.0.0.0
       password: secret
       priv: '*.*:ALL,GRANT'

![edit rol](https://user-images.githubusercontent.com/79808404/202916158-36d2a85d-da5a-4f3f-8de1-5d47f83b7f3d.JPG)


![edit role2](https://user-images.githubusercontent.com/79808404/202916901-b4cf66d9-65c8-474c-bf4e-8bfbae47231d.JPG)

![edit confg](https://user-images.githubusercontent.com/79808404/204148274-9c67e3a6-7886-463c-a0d6-00b9d0561417.JPG)


### Step 10
 I initialized git repo in my Jenkins-Ansible server with the below command and created a new branch '_role-feature_' and switched to the branch
      
      git init
      git pull https://github.com/lexjox777/ansible-config-mgt.git
      git remote add origin https://github.com/lexjox777/ansible-config-mgt.git
      git branch roles-feature
      git switch roles-feature
     

![git init](https://user-images.githubusercontent.com/79808404/204148824-49423d8f-5b9c-4745-843b-6b375cc413d6.JPG)

![git add](https://user-images.githubusercontent.com/79808404/204148828-5f2e2dda-1941-45ba-ad78-e2bbb79736ab.JPG)

![git switch branch](https://user-images.githubusercontent.com/79808404/204148831-0a70e015-ecba-4514-8e23-818b77ddc507.JPG)

I created a pull request and merged to main branch on github.
  
      git checkout main
      git merge roles-feature
    
![git merge role feature](https://user-images.githubusercontent.com/79808404/204148841-77d5b71c-0895-4d44-be77-b9728728e1bd.JPG)


## LOAD BALANCER ROLES

### Step 11
   
   I installed **Nginx and Apache** load balancer **role** with the below command
   
     ansible-galaxy install geerlingguy.nginx
                    &
     ansible-galaxy install geerlingguy.apache
     
  
![install roles](https://user-images.githubusercontent.com/79808404/205451356-b9b0fc75-8678-4f49-96f9-8a3cbd572019.JPG)
  
   I moved the installed roles and renamed them by running the command below
        
       mv geerlingguy.nginx/ nginx
       mv geerlingguy.apache/ apache

![renamed the roles](https://user-images.githubusercontent.com/79808404/205451494-4b34c0b2-64fa-4bd5-9cc5-f4e08dd0c71b.JPG)


### Step 12  Congifuring Nginx and Apache role
 I uncommented and editted my configuration file in the **Nginx--default--main.yml** and editted it as shown in the picture below

![uncomment nginx](https://user-images.githubusercontent.com/79808404/205451760-4792498e-6f6d-43cc-b4c0-4ec25f5ad6fa.JPG)


![ediited nginx 1](https://user-images.githubusercontent.com/79808404/205452080-52455636-b174-47f6-ab19-2146531de04a.JPG)


![uncomment ngnx 2](https://user-images.githubusercontent.com/79808404/205451944-a431d3ba-aa97-4d65-b3f4-3c370c149c97.JPG)


![edit nginx 2](https://user-images.githubusercontent.com/79808404/205452086-04950213-972c-44dc-8751-771eaec6bec8.JPG)

![nginx eddit task](https://user-images.githubusercontent.com/79808404/205452399-0df353ca-b7a8-4ed3-a469-1dbafb3fcb4c.JPG)

![nginx update 1](https://user-images.githubusercontent.com/79808404/206932300-54381517-2f56-46f0-a226-2be450f9e3f0.JPG)


![update nginx 2](https://user-images.githubusercontent.com/79808404/206932303-18d47d14-7593-4311-a55c-b03f6c233e76.JPG)
  
  
  I editted my Aoache Role configuration file in the **Apache--default--main.yml** as shown in the picture.
  

![update apache 1](https://user-images.githubusercontent.com/79808404/206932478-5e4f9d3e-95f4-4cba-bb5a-484e7fe6c895.JPG)

I updated my static assignment folder with a **loadbalancer** file and configured as shown below

    - hosts: lb
      roles:
        - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
        - { role: apache, when: enable_apache_lb and load_balancer_is_required }

![loadbalncer file](https://user-images.githubusercontent.com/79808404/206932684-0b753a17-52ab-4058-bab2-83578b0c8520.JPG)


I updated my static assignment folder with a **db.yml** file and configured as shown below
 
  
![db file update](https://user-images.githubusercontent.com/79808404/206932793-26cde623-57bd-404c-99a2-47a72044f7d9.JPG)


I updated my playbooks folder--site.yml file with the configuration below

     - name: Loadbalancers assignment
       hosts: lb
         - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required 

![update site yml](https://user-images.githubusercontent.com/79808404/206933097-bd7615f6-c63c-4562-a7e2-1162aad215c2.JPG)


I updated my uat.yml file in env-vars folder with the below script

 ![update uat file](https://user-images.githubusercontent.com/79808404/206933278-a79e96d2-cecf-4a72-b1b2-72071ae961d4.JPG)


## Step 13
  
I ran my ansible-playbook with the below commmand to confirm if the configuration works as expected
  
    ansible-playbook -i inventory/uat.yml playbooks/site.yml
   
  
  
![final a](https://user-images.githubusercontent.com/79808404/206933675-bbd48218-1959-463f-9d91-f03bf5cbd7f8.JPG)

![final b](https://user-images.githubusercontent.com/79808404/206933694-3088c1fb-bc4e-4b4d-9b81-f22809eea1e3.JPG)





## Challenges encountered when implementing the project

  ### I got the below blocker when I ran my ansible-playbook
    
        ansible-playbook -i inventory/uat.yml playbooks/site.yml
        
  ### Error : 
       
       role 'mysql' was not found in /home/ubuntu/Project-13/playbooks/../static-assignments/roles:/home/ubuntu/ansible-config-artifact/roles:/home/ubuntu/Project-13/playbooks/../static-assignments



 ![error](https://user-images.githubusercontent.com/79808404/210052684-2c14d358-e55a-42cc-bb3a-6da8227b23ba.JPG)


### Solution: 
   After troubleshooting and searching the net for a similar blocker , I finally got a hint from a mentor to include the path in my ansible.cfg role path configuration.
   
     sudo vi /etc/ansible/ansible.cfg
  
then I uncomment and included the role path to the folder
           
     roles_path = /home/ubuntu/Project-13/roles
       
                &
                
     inventory = /home/ubuntu/Project-13/inventory
  
     
 ![solu 0](https://user-images.githubusercontent.com/79808404/210054883-af501bfa-efb7-476e-bb35-1a200b678781.JPG)



 ![solu 1](https://user-images.githubusercontent.com/79808404/210054894-8daf35df-ef47-48bb-8eaa-1a43e0bc3543.JPG)


![solu 2](https://user-images.githubusercontent.com/79808404/210054904-24bca525-f393-4903-817a-ad256e7a557d.JPG)


   












  
