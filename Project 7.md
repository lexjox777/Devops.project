# DEVOPS TOOLING WEBSITE SOLUTION

### STEP 1
I spin up my AWS EC2 instance and created 4 instances in my RedHAT Linux (NFS , WEBSERVER 1, WEBSERVER 2, WEBSERVER 3) and a (DB Server)in my Ubuntu Linux server. 

![instances](https://user-images.githubusercontent.com/79808404/183240848-3c70ce92-6998-4a52-a1f0-20d88ab73b8b.JPG)

### STEP 2
 I created 3 logical volumes of 10Gib each and attached the volumes to the NFS Server.
 
  ![Attach vol](https://user-images.githubusercontent.com/79808404/183241365-48a2b043-233c-4e89-b926-2d168b83ebe4.JPG)


### STEP 3
  I connected to my NFS Server from my Local machine with my EC2 SSH key and run the command _lsblk_ to view all the block devices attached to my NFS Server
    
      
 ![lsblk](https://user-images.githubusercontent.com/79808404/183357600-e44ccda9-7e08-4a17-83f5-532f94a28312.JPG)

### STEP 4
  I used the Gdisk utility to create a  partition  xvdf, xvdg , xvdh on my disk with the command
  
      sudo gdisk /dev/xvdf
      sudo gdisk /dev/xvdg
      sudo gdisk /dev/xvdh
      
  ![gdisk for partition](https://user-images.githubusercontent.com/79808404/183358352-c631ab11-260f-4321-b0b5-ff86a1ccb811.JPG)

and ran the command _lsblk_ to view the partitions created 

  ![partition created](https://user-images.githubusercontent.com/79808404/183359005-61d6e16d-3726-4df0-a89e-4f39938cb003.JPG)



### STEP 6
  I installed the LVM packages using the command 
    
      sudo yum install lvm2 -y
      
  ![sudo install lvm packages](https://user-images.githubusercontent.com/79808404/183359531-6cba4988-486c-45a0-aa97-f1717891bd4c.JPG)

and ran the command below to check for available partition
  
       sudo lvmdiskscan
             &
           lsblk
 
 ![lvmdiskscan](https://user-images.githubusercontent.com/79808404/183359858-44a9924e-101e-4be1-a4c1-e0c389aeb9c4.JPG)
   
  ![partitiion lsblk](https://user-images.githubusercontent.com/79808404/183360878-d756dcf0-8a0c-4ff5-9294-299ce23ba7da.JPG)


### STEP 7
 I created a physical volume with the command below
   
     sudo pvcreate /dev/xvdf1
     sudo pvcreate /dev/xvdg1
     sudo pvcreate /dev/xvdh1
     
  and ran the command pvs to check the physical volumes were created as expected
   
       sudo pvs
  
  ![physical vol](https://user-images.githubusercontent.com/79808404/183424687-373b0493-35fb-4335-b39e-540be6e69e1a.JPG)
   
   ![sudo pvs](https://user-images.githubusercontent.com/79808404/183424733-1344187b-2de4-4c1e-8135-908782a434f2.JPG)

 ### STEP 8
   I used the command vgcreate to create a volume group for my partition with the command below
   
      sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
      
   and ran the command below to check the volume group was created as expected
     
      sudo vgs
      
   ![sudo vgcreate](https://user-images.githubusercontent.com/79808404/183426174-69fc735b-93c9-46f4-8385-caf260376deb.JPG)


### STEP 9
 I created 3 logical volumes apps, logs and opt with the command below
 
     sudo lvcreate -n lv-apps -L 9G webdata-vg
     sudo lvcreate -n lv-logs -L 9G webdata-vg
     sudo lvcreate -n lv-opt -L 9G webdata-vg

and ran the command below to verified that my logical volumes were created as expected
   
     sudo lvs
       &
     lsblk
![lvcreate apps,logs](https://user-images.githubusercontent.com/79808404/183427334-bff1195c-f4aa-47d9-9485-1901552e69aa.JPG)
    
 ![sudo lvs](https://user-images.githubusercontent.com/79808404/183431040-c417f548-1fa8-4780-8962-0d258359e338.JPG)
 
 ![lsblk for lvcreate](https://user-images.githubusercontent.com/79808404/183431078-0240af1b-acfe-4918-982f-0e485e47f6ab.JPG)
 
 
  
  
  







