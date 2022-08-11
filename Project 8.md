# LOAD BALANCER SOLUTION WITH APACHE

### STEP 1
 I created my Ubuntu server EC2 instance in my AWS console and named it Project8-apache-lb and opened http port 80 in my inbound rule

![pro8 ec2](https://user-images.githubusercontent.com/79808404/184095467-bdc1e6c6-0f29-463b-895e-ba92a1a991fc.JPG)

 ### STEP 2
  I installed apache load balancer on my Project8-apache-lb server with the below command

     sudo apt update
     sudo apt install apache2 -y
     sudo apt-get install libxml2-dev
     
   ![sudo update](https://user-images.githubusercontent.com/79808404/184097568-f89d5a00-8d91-4e52-a6fd-26691bed3c24.JPG)

  ![sudo install apache](https://user-images.githubusercontent.com/79808404/184097597-463f774a-47fe-47f2-96d9-83340a6ed111.JPG)

 ![sudo apt get libxml](https://user-images.githubusercontent.com/79808404/184097632-54bfb261-087c-48cc-b811-b06e8f84df61.JPG)

  and I enabled the modules below with the command
        
          sudo a2enmod rewrite
          sudo a2enmod proxy
          sudo a2enmod proxy_balancer
          sudo a2enmod proxy_http
          sudo a2enmod headers
          sudo a2enmod lbmethod_bytraffic
          
   ![enable module](https://user-images.githubusercontent.com/79808404/184099097-9163dc8d-8f12-46b2-ad03-22c34a471ab9.JPG)
 
 ### STEP 3
   I restarted my apache and check the status with the command below
       
        sudo systemctl restart apache2
        sudo systemctl status apache2
   
   ![sudo restart apache](https://user-images.githubusercontent.com/79808404/184099290-1e2f3bb6-d79b-422d-965a-440f3dbcb6e6.JPG)
   
   
   ### STEP 4
   I configured my Loab balancer by using the VI editor and inputing the below script
   
       sudo vi /etc/apache2/sites-available/000-default.conf
       
            &
         
     <Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
   
  ![sudo vi apche](https://user-images.githubusercontent.com/79808404/184114865-04f4276c-59d6-44da-abb0-e7a37e85cde4.JPG)

   
 ![config load balancing](https://user-images.githubusercontent.com/79808404/184114915-17d557c7-36bb-41b3-9a2f-65fb411ac9a1.JPG)

### STEP 5
  I restarted my apache server to make my configuration take effect
     
       sudo systemctl restart apache2
         &
       sudo systemctl status apache2
       
   ![apache restart after config](https://user-images.githubusercontent.com/79808404/184116043-fb0f018d-b27a-4e69-8280-83bcb34ea588.JPG)

 
 ### STEP 6
  I verified that my configuation is working successfully by accessing my Loab balancer public IP from my web browser
    
       http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php
       
   ![verify conf on web](https://user-images.githubusercontent.com/79808404/184116766-2a3697a5-10c8-449e-b232-d411b030446f.JPG)
 
    


   
   
   
   
   
   
   
   
   
   
   
