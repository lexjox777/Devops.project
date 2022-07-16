 PROJECT 6
  # WEB SOLUTION WITH WORDPRESS
  
   #### STEP 1

I.    I started by launching two instances (“webserver” & “database server”) on my AWS console and choosing RedHat Enterprise as my environment.

![1  ec2](https://user-images.githubusercontent.com/79808404/179339960-17a1725e-bda6-46be-b15e-2a3eba9f284e.PNG)

  
ii. I created 3 new volumes and set the size of each **volume to 10GiB** and the availability zone set to **us-east-1c** to be in the same region as my other instances.
 
 ![2 volume](https://user-images.githubusercontent.com/79808404/179339987-56694c5c-f1b5-4a09-8f69-0576d6885e58.PNG)


iii. I attached the volumes created to my _webserver_ instance

![3 attchVol](https://user-images.githubusercontent.com/79808404/179339995-657f4fa3-14b5-4103-af42-e7c436772e12.PNG)
 


iv. I created a new key pair for my instance and connect to my server with the generated SSH key.


### ENVIRONMENT  
    Platform :
          RedHat (Inferred)	
      Platform details:
          RedHat Enterprise Linux 8(HVM)


#### STEP 2

I connected to my server on my terminal with the SSH key generated and run _lsblk_ command to inspect if the 3 newly created block devices are attached to my server. 

![4 to chck vol attch](https://user-images.githubusercontent.com/79808404/179340309-e91317f4-ba8c-43c9-8fdf-946a31a3f914.PNG)
  
#### STEP 3 
I use the _df –h_ command to see all mounts and free space on my server

![5 mount and free space](https://user-images.githubusercontent.com/79808404/179340343-6fd29c80-e6e4-4e7f-9483-93718f810ead.PNG)


#### STEP 4

I use the command _sudo gdisk /dev/xvdf_  to create a partition for each of my blocks devices xvdf, xvdg and xvdh and set the code to **8E00.**  
           
           sudo gdisk /dev/xvdf
           sudo gdisk /dev/xvdg
           sudo gdisk /dev/xvdh

![8 partition3](https://user-images.githubusercontent.com/79808404/179340384-27f55721-eceb-4fbf-bed3-1c9fbf21e056.PNG)


#### STEP 5

I use the command key _lsblk_ to view the newly configured partition on each of disks created.

![9 view conf partition](https://user-images.githubusercontent.com/79808404/179340946-223d1e52-6437-4008-84e2-9cdf09d046d5.PNG)

#### STEP 6

  I installed my physical volume by running the command _sudo yum install lvm2_ 
![10 phys vol install](https://user-images.githubusercontent.com/79808404/179340957-8677c296-0360-4843-9b3d-38fd93056840.PNG)

and checked available partitions by running the command **sudo lvmdiskscan.**

![11 scan partition](https://user-images.githubusercontent.com/79808404/179341153-28afac0a-6b7c-4471-93c9-e140fbe09fa4.PNG)


#### STEP 7

  I marked my 3 newly created disk as physical volume by running the command 

            sudo pvcreate /dev/xvdf1
            sudo pvcreate /dev/xvdg1
            sudo pvcreate /dev/xvdh1
                      
  ![12  pvcreate](https://user-images.githubusercontent.com/79808404/179341184-a40aa276-c21f-4eee-aba1-e183744989b5.PNG)
         
and verified the physical volume created was successful by running the command _sudo pvs_

![13 pvs to check the phys vol created](https://user-images.githubusercontent.com/79808404/179341221-9ef4715b-4ea8-40c5-8733-cfcc985ec315.PNG)


#### STEP 8

  I used vgcreate to add all 3 physical volume (PV) to a volume group and named the Volume group (VG) webdata-vg

          sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
    
  ![14 vgcreate](https://user-images.githubusercontent.com/79808404/179341292-593a1bb8-fed1-46d5-a94a-cea419a67b12.PNG)

    
I verified that the volume group is created successfully by running the command 
    
            sudo vgs

#### STEP 9

  I used lvcreate to create 2 logical volumes app-lv (to be used to store data for the website) and logs-lv (to be used to store data for the logs) and set the PV sizes to 14gib each.
    
         sudo lvcreate -n apps-lv -L 14G webdata-vg
         sudo lvcreate -n logs-lv -L 14G webdata-vg
         
   ![15 lvcreate -n](https://user-images.githubusercontent.com/79808404/179341383-35f0dcff-c496-441c-bace-3c44bb4dd9f3.PNG)

     
I verified that my logical vol was successfully created by running the command sudo lvs

#### STEP 10

  I used mkfs.ext4 to format the logical volumes with ext4 file system
  
  ![16 mkfs ext4](https://user-images.githubusercontent.com/79808404/179341433-1fd821c7-968a-471e-a525-b47d3a384e0b.PNG)


#### STEP 11

 I created 2 directories /var/www/html ( to store website files) and /home/recovery/logs ( to store backup of log data)
 
 ![17 mkfs ext4](https://user-images.githubusercontent.com/79808404/179341567-6ec392d8-8ec7-4775-9596-540b4accd358.PNG)


 #### STEP 12
 
 I mounted  **/var/www/html** on **apps-lv** logical volume by running the command 
 
          sudo mount /dev/webdata-vg/apps-lv /var/www/html/

![18 sudo mount](https://user-images.githubusercontent.com/79808404/179341533-33b277d0-e14c-4c4d-a28e-e30feed4df01.PNG)


#### STEP 13

  I backup all the files in my log directory /var/log into /home/recovery/logs before mounting the file system by running 
  
    sudo rsync -av /var/log/. /home/recovery/logs/
  
![rsync backupfiles](https://user-images.githubusercontent.com/79808404/179342516-72b15328-2793-46c9-bee5-7144101cc4f2.PNG)

and checked the backup files are backed-up as expected by running 
  
    sudo ls -l /home/recovery/logs
    
  ![check backup on recovery](https://user-images.githubusercontent.com/79808404/179343279-65cff6c5-86ea-49e5-a95a-0b8ae548c6a3.PNG)


#### STEP 14
 
   I mounted /var/log on logs-lv logical volume by running the command below
       
          sudo mount /dev/webdata-vg/logs-lv /var/log
   
   ![12  mount var log on logs v](https://user-images.githubusercontent.com/79808404/179343018-3a6541a8-86fb-4a8a-9508-b0d3ab6c7b24.PNG)

#### STEP 15

 I restored log files back into /var/log directory by running the below command
     
     sudo rsync -av /home/recovery/logs/. /var/log


![retore log files](https://user-images.githubusercontent.com/79808404/179343893-b1409b00-b68a-4f1e-bd67-d2ab688631dd.PNG)

#### STEP 16
 
 After restoring the backed-up files back into the /var/log directory I ran the below command to be sure that the files are restored as expected
    
      sudo ls -l /var/log

![check if file restored](https://user-images.githubusercontent.com/79808404/179344076-273c5f5d-044f-46c2-81d8-0f1a44012582.PNG)



#### STEP 17
  I ran the command _sudo blkid_ to check my **block id** to populate my fstab

![sudo blkid](https://user-images.githubusercontent.com/79808404/179345219-4386e98c-0a43-4315-926d-2b9cda70499b.PNG)

I insert the _block id_ in the _fstab_ by running the command
  
    sudo vi /etc/fstab
    
![fstab insert](https://user-images.githubusercontent.com/79808404/179345316-3765240c-2602-467b-b3ad-dd9206d853eb.PNG)

I checked if my configuration works prfectly with _sudo mount -a_ command

![sudo mount -a](https://user-images.githubusercontent.com/79808404/179345359-3f198e5f-2a16-488d-b881-a47275bc1a14.PNG)

I reloaded the daemon to see if my configuration persist with the below command
   
    sudo systemctl daemon reload
  
![sudo daemon reload](https://user-images.githubusercontent.com/79808404/179345472-259db749-e870-4d51-abf8-9b4e352eed01.PNG)


I used the command _df -h_ to see if my configuration is updated 

![to be sure fstab updated](https://user-images.githubusercontent.com/79808404/179345538-67ac1210-f035-4513-9b26-96d9172e54e2.PNG)
















