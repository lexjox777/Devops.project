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
 
 ![lvmdiskscan](https://user-images.githubusercontent.com/79808404/183359858-44a9924e-101e-4be1-a4c1-e0c389aeb9c4.JPG)
   
  









