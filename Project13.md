# Introducing Ansible Dynamic Assignments (Include) and Community Roles into my Structure

Step 1
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


Step 2
  I Created a new git branch dynamic-assignments 

![2-git branch](https://user-images.githubusercontent.com/79808404/201151477-808cb54e-d66c-4213-b938-8f647a4532ff.JPG)


Step 3
   I created new folder **env-vars** and the below files within the env-vars folder.
      
        ├── env-vars
          └── dev.yml
          └── stage.yml
          └── uat.yml
          └── prod.yml
      
   
   ![3-new folder vars](https://user-images.githubusercontent.com/79808404/201152230-170b89c0-940b-4c74-87b3-7c446dc3b554.JPG)


Step 4
  
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


Step 5
   I commited all the updated changes to my Github
     
   
   ![5 - git add files](https://user-images.githubusercontent.com/79808404/201154777-d2c49fac-9b45-4648-b48f-c7fb32af778a.JPG)

Step 6
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


Step 7
  
  I commited the changes to my Github 
    
![7 - git add update](https://user-images.githubusercontent.com/79808404/201156185-749d9a11-39e1-4837-af84-a541316141e7.JPG)

# Step 8
  I installed mysql role from ansible-galaxy community with the below command
    
     ansible-galaxy install geerlingguy.mysql
     
   ![1  mysql role](https://user-images.githubusercontent.com/79808404/202915076-62e85a70-d9f4-449e-b113-3dc34f4bf9ef.JPG)


![2  mysql role](https://user-images.githubusercontent.com/79808404/202915082-f11d812f-828e-47fc-8dd4-d945ed8fb8f9.JPG)



![3 mysql role created](https://user-images.githubusercontent.com/79808404/202915090-b04127eb-f336-49e6-b0f0-201f77ea6b92.JPG)






  
