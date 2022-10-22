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

   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
