#  **ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)**

 ## Project scope and architecture
  
  ![project arch](https://user-images.githubusercontent.com/79808404/197327592-9b79d643-7903-43b1-b2bf-380220030d5d.JPG)


## _Jenkins Job Enhancement_

 ### Step 1
    In my Jenkins server, I created a new directory _ansible-config-artifact_ and changed permission to the directory so my Jenkins could save files in there.
    
            sudo mkdir /home/ubuntu/ansible-config-artifact

                                &
            
               chmod -R 0777 /home/ubuntu/ansible-config-artifact



![mkdir artifat](https://user-images.githubusercontent.com/79808404/197327073-ecaa6f14-d88e-49f4-8c4b-cb0fca66e793.JPG)
