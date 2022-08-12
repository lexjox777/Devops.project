# Implementing a Load Balancer Web Solution for Devops Team
   
   ### Project Architecture and Goals
    
  In this project we will enhance our Tooling Website solution by adding a Load Balancer to distribute traffic between Web Servers and allow users to access our website using a single URL.
    
  ![proj arch](https://user-images.githubusercontent.com/79808404/184222130-c7e16af6-6270-4f3a-b847-fa52725b1de1.png)







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
 
    
### STEP 7
  I ran the command below on my Web Servers to view access logs of each Webservers receiving HTTP GET requests from my Load Balancer and confirming a new logs in each web servers each time the web browser page is refreshed.   
  
       sudo tail -f /var/log/httpd/access_log
       
  ![access log for web1](https://user-images.githubusercontent.com/79808404/184288441-883eafe2-eb11-482b-b1ee-4d8080d2f6ea.JPG)

![web2 log](https://user-images.githubusercontent.com/79808404/184288455-6fe2b9c3-cd36-4354-9b20-53d95c9c25f3.JPG)



## Optional Steps-  Configuring Local DNS Names Resolution
  ### Configuring my IP address to domain name mapping my Load Balancer
  
  #### STEP 1
   I opened the file below in my Load Balancer server and input my each web servers Private IP and give an arbitrary name
   
       sudo vi /etc/hosts  
     
        &
      <WebServer1-Private-IP-Address> Web1
      <WebServer2-Private-IP-Address> Web2  
      
![sudo vi host](https://user-images.githubusercontent.com/79808404/184290437-2ea6dd12-96dd-41c4-b267-1d0d2ce305ef.JPG)

   
 ![hosts web1 web2](https://user-images.githubusercontent.com/79808404/184290448-87d04f16-2550-44b1-bd65-cb29ca0c6903.JPG)

#### STEP 2
   I updated my Load Balancer config file IP addresses to the above input 
       
        sudo vi /etc/apache2/sites-available/000-default.conf
          
                &
                
         BalancerMember http://Web1:80 loadfactor=5 timeout=1
         BalancerMember http://Web2:80 loadfactor=5 timeout=1
         
       
  ![web1web2](https://user-images.githubusercontent.com/79808404/184292067-2e4de9de-4f13-4660-ba32-bae972612d22.JPG)
       
   
#### STEP 3
  I used the curl command to curl my web servers from my Load balancer server locally with the below command
     
       curl http://Web1
       curl http://Web2
       
       
  ![curl web1](https://user-images.githubusercontent.com/79808404/184292121-7c41cd14-dd77-411d-be53-1f6527c55467.JPG)
  
  ![curl web2](https://user-images.githubusercontent.com/79808404/184292126-37179c3b-8e9a-4732-afbd-830865988028.JPG)
    
     
   
   
   
   
