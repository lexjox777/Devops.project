# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

#  Implementing an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.


## Project Architecture 
![load arch](https://user-images.githubusercontent.com/79808404/188480776-ab82641a-b81d-4892-9a38-a9ccb63f2ddf.png)

### Step 1
  Created a domain name **techlect.co.uk** from Godaddy.com
 
 ![domain](https://user-images.githubusercontent.com/79808404/188496945-c62d7b62-3c12-4c47-ad5b-613894d8fde3.JPG)


### Step 2
 Hosted my domain name on AWS **Route 53** by navigating to the _create hosted zone_ tab
 
 ![route 53](https://user-images.githubusercontent.com/79808404/188502539-9bae568d-02d9-4756-a492-7b7dfd28e011.JPG)

I input my domain name and create hosted zone.
 
 ![create hosted zone](https://user-images.githubusercontent.com/79808404/188503165-62989e14-2c49-4a77-b3b0-dd4d8cca035e.JPG)

 ![techlct hosted created](https://user-images.githubusercontent.com/79808404/188503599-1492a06f-2aa2-4f6c-af5e-06cc3e6d823f.JPG)
 
 
 ### Step 3
  
   From my domain management tool, I edited the default nameserver to my AWS nameserver.
   
   ![create record](https://user-images.githubusercontent.com/79808404/188624422-0ac8fd49-3f22-47df-81fe-dcb21a11c361.JPG)

   
   ![name server](https://user-images.githubusercontent.com/79808404/188624378-35d65ed7-9cef-4b6b-83bf-c49b28a31398.JPG)
   
   
   ### Step 4
   From my AWS console I created a new instance _LoadBalancer_  and spinned up the instance
   
   ![loadbal](https://user-images.githubusercontent.com/79808404/188635431-b0bb8f46-6b7b-40ed-a8bc-b08fdd58e2ed.JPG)
   
   
   ### Step 5
   
   From my **AWS Route 53** I navigated to the _create record_ tab to create 2 records and input my loadbalancer Public IP  address and subdomain 'www'
   
   ![create recordd](https://user-images.githubusercontent.com/79808404/188641269-a14d30dd-b5a0-4a5e-8f22-e4cd0e994a38.JPG)

 
   ![Ip input](https://user-images.githubusercontent.com/79808404/188642363-a750a996-2d0b-4d0d-a7a4-a4f7dc8eccd2.JPG)
 
 
   ![record creatd](https://user-images.githubusercontent.com/79808404/188644179-16663d90-bc68-40b5-b1f1-1a1a716a4dd3.JPG)

   
   ![create record2](https://user-images.githubusercontent.com/79808404/188646140-5a7df990-5570-49e2-b110-5a9be835a054.JPG)

   
   ### Step 6
   I connected my Loadbalancer ssh key on my terminal and ran the command _sudo apt update_ to update all outdated dependacies and _sudo apt install nginx -y_ to install nginx on my loadbalancer server.
   
      sudo apt update
      sudo apt install nginx -y
   
   ![sudo install nginx](https://user-images.githubusercontent.com/79808404/188657856-07d951b5-a713-49da-8795-f64d1f204a05.JPG)

   
 I used the command below to start nginx and enabled it on my server
      
       sudo systemctl enable nginx
       sudo systemctl start nginx
       sudo systemctl status nginx
       
   ![enble nginx](https://user-images.githubusercontent.com/79808404/188659447-bee40ad5-22d5-4837-9b44-1484c10f6a5e.JPG)

  ### Step 7
   I used the command below to vi into my loadbalancer 
     
       sudo nano /etc/nginx/sites-available/load_balancer.conf
       
   and configured it with the script below by inputing my web1 and web2 Public Ip addresses and my Domain name.
   
       upstream myproject {
         server Web1 weight=5;
         server Web2 weight=5;
    }

      server {
         listen 80;
         server_name www.domain.com;
         location / {
           proxy_pass http://myproject;
      }
    }

    #comment out this line
    #       include /etc/nginx/sites-enabled/*;
        
        
   ![loadb conf](https://user-images.githubusercontent.com/79808404/188672254-272053de-a48a-4f61-a5b7-4748e4d51b7b.JPG)

   
   
   I used the command below to remove my default site so that my reverse proxy will be redirecting to my newly configuration file.
     
        sudo rm -f /etc/nginx/sites-enabled/default
        
   ![cap](https://user-images.githubusercontent.com/79808404/188675383-48d152a2-b399-4993-b219-61c0db8f750e.JPG)

   
   
   Then used the below command to test if my configuration is successful
      
         sudo nginx -t
     
   ![test nginx](https://user-images.githubusercontent.com/79808404/188684773-757421cb-f6fc-44f5-8af6-9c6927939f62.JPG)

   
   
   
   
   
   
   
   
   

