# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY

![use case](https://user-images.githubusercontent.com/79808404/233130551-32a86a0a-7555-42b5-84ad-da54d6e01735.JPG)
 
 ** _This whole infrastructure is implemented manually_
  **
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
 
 
 
 _VPC : Amazon Virtual Private Cloud  is a web service that allows users to create a virtual private cloud in Aws environment. 
  Aws VPC provides features like subnets, security groups, networks ACLs which allows user to control traffic flows and security of their resources._


_Route53 DNS : this an AWS DNS service that helps users to manage their domain names and route internet traffic to thier infrastructure on AWS or elsewhere. 
with route53 users can  register domain names, manage DNS records, and route traffic to various AWS and non-AWS resources._

_ALB : Application load balancer helps to distribute incoming traffic accross multiple targets such as Elastic compute cloud(EC2) instances,containers and IP addresses_


_Internet gateway IGW : securely and efficiently connects the internet external network to the resources located in the virtual private cloud_



_A reverse proxy server is a type of proxy server that sits behind the firewall in a private network and directs clients request to the appropriate backend server.
Load balancing reverse proxy acts as a traffic cops sitting in front of a backend server and distributing client requests across group of servers in a manner that maximizes speed and capacity._


_Auto scaling: allows users to automatically scale their compute resources up or down based on application demand. Auto scaling works by automatically adding or removing instances based on predifined policies and threshold which can be based on metrics such as CPU usage, network traffic or application response time._



_Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic Network File System (NFS) for use with AWS Cloud services and on-premises resources. In this project, we will utulize EFS service and mount filesystems on both Nginx and Webservers to store data._






-----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------



 ### Step 1. I created a Virtual Private Cloud(VPC) in my AWS console and named it "VPC-ACS"
 
 
  ![1  VPC](https://user-images.githubusercontent.com/79808404/233317555-b348bcc2-b68f-4689-8c0e-b3ff237f612f.JPG)

 
 
  ![2  Create VPC](https://user-images.githubusercontent.com/79808404/233317805-ba03acea-178a-4125-8f49-31e74f08f769.JPG)



 ### Step 2. I edited my VPC to enable DNS hostname
 
 ![3 To enable DNS hostname](https://user-images.githubusercontent.com/79808404/233318605-daab9374-a132-4f96-8a1c-66a830cc7b43.JPG)


 ![3b](https://user-images.githubusercontent.com/79808404/233318640-f0fd2d61-f330-4477-849e-a717330a00ab.JPG)



 ### Step 3. I created an internet gateway and attached it to my VPC. This will allow and enable communucation between the resources within my VPC and the internent.
 
  ![4  create internent gateway for my VPC](https://user-images.githubusercontent.com/79808404/233319163-46a3071b-7aae-4a4d-ae37-d2da04df0c81.JPG)


![4b](https://user-images.githubusercontent.com/79808404/233319284-f4209beb-b427-4a4e-9aae-f705e75de66e.JPG)


![5  Attached the Igw to my VPC](https://user-images.githubusercontent.com/79808404/233319352-869f64e4-ab0c-433b-98ab-774b32ac45b3.JPG)


![5b](https://user-images.githubusercontent.com/79808404/233319484-5e79855d-1aea-46e3-91fa-284a87e33cff.JPG)

 
![5cc](https://user-images.githubusercontent.com/79808404/233319507-b5b1a0ab-2b5c-48aa-94be-1d4e1941470d.JPG)



### Step 4. I created subnets (private and public subnets) This allows better organization and well management of my resouces, improved network security(by distinguishing between a public subnet such as webserver and a subnet that requires a private access like databases) and also improve network availabilty by spreading across multiple Availabilty Zones.


![6  create subnets](https://user-images.githubusercontent.com/79808404/233322643-5f546a42-d1d3-494b-ac56-b1ecafc2c7b4.JPG)


![6b  public subnet 1 created](https://user-images.githubusercontent.com/79808404/233322689-940f0344-b310-48b9-89eb-cc1d40bfaeb9.JPG)


![6c  public subnet 2 created](https://user-images.githubusercontent.com/79808404/233322708-f1ceebd5-62de-49af-bdf7-215ff1fa0163.JPG)


![7  private subnet 1 created](https://user-images.githubusercontent.com/79808404/233322823-2c971476-014a-47a6-a523-99fd049a0ac0.JPG)


![7b  private subnet 2 3 created](https://user-images.githubusercontent.com/79808404/233322981-42ff4a9b-f3a9-4df5-b9f0-7e2209657404.JPG)

![7c  private subnet 4 created](https://user-images.githubusercontent.com/79808404/233323010-cd01566d-9496-4bbf-8fb2-138cae459b62.JPG)



### Step 5. I created Route tables( Private and Public route tables) and associated it with my subnets

  
![8  created route table](https://user-images.githubusercontent.com/79808404/233325533-d2c64df8-b500-4797-b9b4-21283dfdd67e.JPG)


![8b  public route table created](https://user-images.githubusercontent.com/79808404/233325570-4f4bf262-7711-45b3-8bd6-32278dacd7e8.JPG)

![8c  private route table created](https://user-images.githubusercontent.com/79808404/233325813-20908fbb-6840-4049-bd93-972e913d743c.JPG)


![9  associate public rtb to public subnet](https://user-images.githubusercontent.com/79808404/233326005-788c9679-08b1-4962-8e61-5674121f0545.JPG)


![9b](https://user-images.githubusercontent.com/79808404/233326072-ce8f99c7-088b-47bf-9fbc-d0d4d68e43e2.JPG)


![10  associate private rtb to private subnet](https://user-images.githubusercontent.com/79808404/233326123-e7850293-ab74-4e03-8375-11638e96b57d.JPG)


![10b](https://user-images.githubusercontent.com/79808404/233326150-7c744274-5ed8-4dd3-9038-a6aff6963ce1.JPG)



### Step 6. I edited the private and public route table to to target my IGW

![11  edit public rtb](https://user-images.githubusercontent.com/79808404/233328999-cf24012d-8ede-4859-ad9e-eb0d96d5dcc4.JPG)

![11b](https://user-images.githubusercontent.com/79808404/233329033-3422ad62-02ee-492f-9d39-967e7c91bf33.JPG)



### Step 7. I created and allocated an Elastic IP for NAT Gateway within my VPC to allow instances in Private Subnets to access the internet without exposing their IP addresses and still keeping them protected from incoming traffic from the internet.

![12  allocate elastic ip for NAT gateway](https://user-images.githubusercontent.com/79808404/233337956-221cb642-0b66-40ae-a5b2-181011cfbc90.JPG)

![12b](https://user-images.githubusercontent.com/79808404/233337993-f478e9a4-0bbd-4c55-aa62-39ec8e23cbaa.JPG)


![13  Create a NAT Gateway](https://user-images.githubusercontent.com/79808404/233338011-83f9b31d-7d1e-4159-a5d4-531c7ca4d401.JPG)


![13b](https://user-images.githubusercontent.com/79808404/233338080-7ad5e025-4dcd-45a4-8d71-9fc4dcc1a439.JPG)


![14  Edit private rtb routes](https://user-images.githubusercontent.com/79808404/233339420-2e2d15b4-8817-4296-848f-d59704593196.JPG)


![14b](https://user-images.githubusercontent.com/79808404/233339444-cd05568f-bc58-462d-aef0-089eb9904de2.JPG)


### Step 8. I created security groups for all my resources to control inbound and outbound traffic to and from the resources within my VPC


![15  create security group](https://user-images.githubusercontent.com/79808404/233341885-7ed0a742-259c-4582-bc0c-6b6537788be8.JPG)


![15b](https://user-images.githubusercontent.com/79808404/233341909-bb15747b-2f35-487f-b19c-d5d2cddbbe7b.JPG)


![16  create security group for bastion](https://user-images.githubusercontent.com/79808404/233341925-9f557e60-71cc-4fa0-812f-519eb7899075.JPG)

![17  create security group for nginx](https://user-images.githubusercontent.com/79808404/233341997-ef8dcf59-c666-42be-a3ad-f27f0ffc6fc0.JPG)


 ![17b  additional rule](https://user-images.githubusercontent.com/79808404/233342044-39ee3275-199a-418a-b399-acce13d1ae72.JPG)

 ![18  create security group for int ALB](https://user-images.githubusercontent.com/79808404/233342068-3c0b80b2-02e3-4369-a579-a83e794f110d.JPG)

 ![19  create security group for webservers](https://user-images.githubusercontent.com/79808404/233342091-0e11675c-ce2a-4bf2-84c1-5c8140281d77.JPG)

 ![20  create security group for datalayers](https://user-images.githubusercontent.com/79808404/233342117-21d4c561-fdc9-4f15-907c-58b235503090.JPG)



### Step 9. I requested for a certificate from ACM (Amazon Certificate Manager) to secure my website traffic and other network communications associated with my website.

 
 ![21  aws certificate manager](https://user-images.githubusercontent.com/79808404/233345292-7310b0b0-9d8a-4eb1-9d80-62b28a9411bc.JPG)

 ![21b  cert request](https://user-images.githubusercontent.com/79808404/233345328-c2e9c35b-0a9e-496a-aad7-de8fe6602f81.JPG)

 ![21c create record in route 53](https://user-images.githubusercontent.com/79808404/233348938-5b99bba4-e327-441f-9e67-5b7e66c549ad.JPG)

 
 ### Step 10. I created Elastics file system (EFS) to store and access files from multiple instances within my VPC.
 
 ![22  create EFS](https://user-images.githubusercontent.com/79808404/233352192-cab9e80f-dbfa-4a94-8952-5c1e0f2576e1.JPG)

 ![22b  create EFS](https://user-images.githubusercontent.com/79808404/233352224-a92fabaf-51f0-4af6-909d-6cac3db5c4d2.JPG)

 ![22c  create EFS](https://user-images.githubusercontent.com/79808404/233352243-369f6444-9707-49f3-bd2a-76f3c30d1f28.JPG)


### Step 12. In AWS Elastic File System(EFS) I created an access-point for both wordpress and tooling which will helps to improve security and access control for data stored in my EFS system.

![23  to create access point](https://user-images.githubusercontent.com/79808404/233355661-546425f8-5e40-4c40-882c-436723b48dd2.JPG)


![23b  creating access point for wordpress](https://user-images.githubusercontent.com/79808404/233355684-dc91c67b-b0e7-49fd-85af-f7921fdb8b28.JPG)


![23c  create access point](https://user-images.githubusercontent.com/79808404/233355701-89c2c164-c1b9-45c1-be4c-e1df1ae2fa77.JPG)


![24  create access point for tooling](https://user-images.githubusercontent.com/79808404/233355724-11839a5a-8aa2-4e70-905c-678858c1c243.JPG)

![24b create access point for tooling](https://user-images.githubusercontent.com/79808404/233355775-b6977b49-82d4-476e-acc3-6cda16b805a2.JPG)


### Step 13. I created and configured a Key Management Service (KMS) to encrypt my data to ensure it cannot be accessed by unauthorised user. this improves data security, meet compliance requirements, secure data transfer and provide access control for my data.

 ![25  create key management service](https://user-images.githubusercontent.com/79808404/233365502-cb0a682d-0972-493a-9d0f-45822939a368.JPG)

![25a  KMS configuration](https://user-images.githubusercontent.com/79808404/233365545-21b91679-8276-467a-a94f-9c5a37003942.JPG)

![25b](https://user-images.githubusercontent.com/79808404/233365697-4f4b4436-b1ff-4497-8b5c-afd1d4f051b8.JPG)

![25c](https://user-images.githubusercontent.com/79808404/233365724-86c5788c-653a-41b3-90b0-2d44e61e46fd.JPG)



### Step 14. I created Relational Database Service(RDS) subnet group to specify the subnets in which my database instance can be launched.


![26  to create RDS subnet groups](https://user-images.githubusercontent.com/79808404/233368593-50a65544-5e29-4bc8-830a-08f46412a6c9.JPG)



![26b  creating rds subnet group](https://user-images.githubusercontent.com/79808404/233368614-adb53421-6b24-47ae-b276-42c1df1f18c4.JPG)

### Created a database for my infrastructure.

![27  creating database](https://user-images.githubusercontent.com/79808404/233369098-cda8a1ea-3996-4b55-986d-1b89a7d3b860.JPG)


![27b](https://user-images.githubusercontent.com/79808404/233369161-09e97df8-70e7-4e12-8d99-09505a967236.JPG)


![27c](https://user-images.githubusercontent.com/79808404/233369195-a52fc52b-07cc-4b79-82d6-e687ed68ad41.JPG)


![27d](https://user-images.githubusercontent.com/79808404/233369313-5eed29c1-d47d-43ff-88f1-c750c394fa1e.JPG)


![27e](https://user-images.githubusercontent.com/79808404/233369342-eeb5516a-f070-4efb-ac30-61ec4fd5fe01.JPG)


### Step 15. I created and configured Three Redhat instances(Bastion, Nginx and Webserver) in my VPC.

![28  created 3 red hat instances](https://user-images.githubusercontent.com/79808404/233380323-e34036ee-90ea-448e-b575-12c8c9543e8a.JPG)

 ### _Bastion configuration_
 
    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm 
    yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm 
    yum install wget vim python3 telnet htop git mysql net-tools chrony -y 
    systemctl start chronyd 
    systemctl enable chronyd


![29  config bastion](https://user-images.githubusercontent.com/79808404/233380400-b7e5767c-f183-4ac4-8cc9-57c27a2316b7.JPG)

![29b  config bastion (python install)](https://user-images.githubusercontent.com/79808404/233380433-3b07b6d7-45e2-4a24-8cd2-f6cabb79abbd.JPG)

![29c  config bastion](https://user-images.githubusercontent.com/79808404/233380449-fff3dc89-df20-4039-9ff3-1035b1c0f4f0.JPG)


### _Nginx configuration_

    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

    yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

    yum install wget vim python3 telnet htop git mysql net-tools chrony -y

    systemctl start chronyd

    systemctl enable chronyd



![30  config nginx ami](https://user-images.githubusercontent.com/79808404/233380477-7432f7fc-9613-4730-b64d-1f5707530bb4.JPG)

![30b  config nginx ami (python install)](https://user-images.githubusercontent.com/79808404/233380504-3e677bd3-d2b1-4cbe-9333-ea55cda52927.JPG)

![30c config nginx ami(start and enable)](https://user-images.githubusercontent.com/79808404/233380531-0a76adfe-9d8d-46d1-b743-d44f29347f0a.JPG)

   ### _Set policies for Nginx_
     
     setsebool -P httpd_can_network_connect=1
     setsebool -P httpd_can_network_connect_db=1
     setsebool -P httpd_execmem=1
     setsebool -P httpd_use_nfs 1
     
     
![30d  config nginx ami (set policies)](https://user-images.githubusercontent.com/79808404/233380581-d0590331-a35a-4123-bece-4f36a561ec33.JPG)



### _EFS Utils_

    git clone https://github.com/aws/efs-utils

    cd efs-utils

    yum install -y make

    yum install -y rpm-build

    make rpm 

    yum install -y  ./build/amazon-efs-utils*rpm

![31  install amazon efs utils](https://user-images.githubusercontent.com/79808404/233380613-a31a1ed0-a996-400b-ba03-c526e440108f.JPG)


### _Installing self signed cert_

     sudo mkdir /etc/ssl/private

     sudo chmod 700 /etc/ssl/private

     openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ACS.key -out /etc/ssl/certs/ACS.crt

     sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

![32  install self signed cert](https://user-images.githubusercontent.com/79808404/233380656-9b59b417-1f45-4e4a-bdd3-bb1e47963b1d.JPG)


![32b  install self signed cert](https://user-images.githubusercontent.com/79808404/233380689-fe1d8479-b96a-4a75-9275-d1d85d29f9a1.JPG)


![32c  install self signed cert](https://user-images.githubusercontent.com/79808404/233380736-018ffb89-6852-43c9-9097-74e7a0de724c.JPG)


### _Configuring webserver_

    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

    yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

    yum install wget vim python3 telnet htop git mysql net-tools chrony -y

    systemctl start chronyd

    systemctl enable chronyd
 

![33  config webserver ami](https://user-images.githubusercontent.com/79808404/233380776-0f3f8679-5780-4d01-9039-c685f54ac410.JPG)


![33b  config webserver ami](https://user-images.githubusercontent.com/79808404/233380809-edeb62f9-5b03-43cd-9e75-5b5aa57cc6b1.JPG)


![33c  config webserver ami (enable and start )](https://user-images.githubusercontent.com/79808404/233380880-55f6ccc5-ceb4-4e3c-81b7-46c0158dc523.JPG)


![34  config webserver( selinux policies)](https://user-images.githubusercontent.com/79808404/233380917-d96d5f99-1989-4938-8270-75a6d75f6c3e.JPG)


![35  config webserver (install amazon efs utils)](https://user-images.githubusercontent.com/79808404/233380939-d002fd95-0b94-4858-8a3a-df8db94d83b9.JPG)


### _Installing self signed cert for apache webserver_

     yum install -y mod_ssl

     openssl req -newkey rsa:2048 -nodes -keyout /etc/pki/tls/private/ACS.key -x509 -days 365 -out /etc/pki/tls/certs/ACS.crt

     vi /etc/httpd/conf.d/ssl.conf
     
     
![36  install self signed cert for apache webserver](https://user-images.githubusercontent.com/79808404/233380971-b86bc33b-a571-4231-a26c-e395e289f063.JPG)

 
 ![36b  install self signed cert](https://user-images.githubusercontent.com/79808404/233381044-11e88aaf-2c91-48af-be66-0460d6ff7f80.JPG)

 
 ![36c  edit localhost to acs crt](https://user-images.githubusercontent.com/79808404/233381081-f6787561-8589-49e1-88f2-9aaf9ec5223f.JPG)

 ![36c](https://user-images.githubusercontent.com/79808404/233381119-d411767c-6b00-4c98-8946-fa39f1c8bb72.JPG)
 
 
 ### Step 16. I created _Amazon Machine Images (AMI)_ for my resources (Webserver, Bastion and Nginx)
 
 ![37a  creating AMI for bastion](https://user-images.githubusercontent.com/79808404/233401995-07e9def6-f9e2-4c38-9168-ed320f9f4eff.JPG)


![37b](https://user-images.githubusercontent.com/79808404/233402046-dbf53a3b-70e9-4e7f-8c68-629652251e43.JPG)


![38  creating ami for webserver](https://user-images.githubusercontent.com/79808404/233402084-91d3499f-9d6d-4ce2-af2c-f7a160fd9aa9.JPG)


![39  creating ami for nginx](https://user-images.githubusercontent.com/79808404/233402118-97d37de4-4078-4aa8-b96a-41378343d9fb.JPG)


### Step 17. I created _Target Groups_ for my resources (Webserver, Bastion and Nginx)

![40  creating target group](https://user-images.githubusercontent.com/79808404/233402152-3135f583-7890-4249-8e51-ce1dc6d58fdd.JPG)

![40a  target group for nginx](https://user-images.githubusercontent.com/79808404/233402222-b1637ab5-d856-4cbf-8a74-4082c317f5c6.JPG)

![41  target group for wordpress](https://user-images.githubusercontent.com/79808404/233402267-e98c7ab1-8b54-42a8-9261-e0df1a45da2c.JPG)

![42  target group for tooling](https://user-images.githubusercontent.com/79808404/233402317-3756fbe0-b77d-4e7c-bff9-c1d8dc37fae7.JPG)

### Step 18. I Created int and ext Loadbalancer to control traffic in my VPC


![43 creating loadbalancer](https://user-images.githubusercontent.com/79808404/233407324-93f484cc-eea4-4711-b06e-db26e3b1c9db.JPG)

![43b](https://user-images.githubusercontent.com/79808404/233407350-b24c4f81-e588-4ae6-9e32-d6bf344f3812.JPG)

![43c](https://user-images.githubusercontent.com/79808404/233407387-b66d19ee-3c84-49c4-a793-3271d35251df.JPG)

![43d](https://user-images.githubusercontent.com/79808404/233407451-6e2ed7b9-4de8-4cc3-8fd1-ed03580c7df6.JPG)

![43e](https://user-images.githubusercontent.com/79808404/233407719-e554e2fe-ec20-496e-8a65-47678af1b52a.JPG)

![43f](https://user-images.githubusercontent.com/79808404/233407794-1e36deab-f40f-4e3b-8829-6bd380455ef2.JPG)

![44a  creating int loadbalancer](https://user-images.githubusercontent.com/79808404/233407847-53d85766-d584-4db2-81a0-ebcc6451c8c5.JPG)

![44b](https://user-images.githubusercontent.com/79808404/233407902-b02a76b8-4a61-457c-b214-9077f289b66f.JPG)

![44c](https://user-images.githubusercontent.com/79808404/233407945-5fddf40c-a6f3-4db7-bd02-aacfa3a4eadd.JPG)

![44d](https://user-images.githubusercontent.com/79808404/233407975-dc68a84e-506c-4596-90f4-b50fd5264b31.JPG)

![45  edit rules in int ALB](https://user-images.githubusercontent.com/79808404/233408033-01bd0159-95fa-499e-aa8c-2f50c90d0e04.JPG)

![45b](https://user-images.githubusercontent.com/79808404/233408061-29b0cc44-88d7-4a58-8ca3-004cd5fcacd5.JPG)


### Step 19. I created Launch Template for all my resources(Bastion, Webserver, Nginx, tooling)

### _Create Launch Template For Bastion_
![46 launch template](https://user-images.githubusercontent.com/79808404/233422415-59d0f452-ba01-49cf-b750-7ef12bc3516e.JPG)

  ![46b  creating launch template for bastion](https://user-images.githubusercontent.com/79808404/233422447-e2eef3d7-a446-408f-a32b-b0437d775578.JPG)

![46c](https://user-images.githubusercontent.com/79808404/233422468-ef869c95-cac2-402e-bf36-d031a739b78b.JPG)

![46d](https://user-images.githubusercontent.com/79808404/233422543-0455bce3-afef-4923-8619-c2df5aee1f1d.JPG)


![46e](https://user-images.githubusercontent.com/79808404/233422576-53ac9e55-8901-4f28-ad60-ee47f343ceca.JPG)

### _Create Launch Template For Nginx_
![47  creating lauch template for nginx](https://user-images.githubusercontent.com/79808404/233422613-37d4ce55-7594-4860-868c-7555ee602764.JPG)

![47b](https://user-images.githubusercontent.com/79808404/233422630-28181445-ee53-4dbb-a70c-f4a176daf941.JPG)

![47c](https://user-images.githubusercontent.com/79808404/233422658-afea8919-306e-4131-8622-a1eb0e0a5923.JPG)

![47d](https://user-images.githubusercontent.com/79808404/233422685-3399317f-096f-4b48-a23b-8c9a4a7467bd.JPG)


![48  copied int ALB dns](https://user-images.githubusercontent.com/79808404/233422857-4bef4283-c097-4bb2-8cbc-e80be7fb02fa.JPG)


### _Create Launch Template For Wordpress_
![49  create launch template for wordpress](https://user-images.githubusercontent.com/79808404/233422884-4d79d458-181c-43fc-91cf-1749c6bcdd77.JPG)

![49b  copied the wordpress mount point in EFS](https://user-images.githubusercontent.com/79808404/233423625-3c6ea0d5-91d1-42f4-8758-ecc737440782.JPG)

![49c  click attach to copy the mount point](https://user-images.githubusercontent.com/79808404/233423650-20d53d94-0616-4f1b-8a9c-40183e087e86.JPG)

![49d  copy mount point](https://user-images.githubusercontent.com/79808404/233423700-48b6bd48-9115-4f03-a7eb-9a774d649bba.JPG)


![49e  mounted the efs accesspoint](https://user-images.githubusercontent.com/79808404/233423737-11486b2c-a3e4-485c-83e2-456c7c052992.JPG)

![49f  copied my database endpoint](https://user-images.githubusercontent.com/79808404/233423768-a5314e1a-8fec-4f88-ae7d-ee72c4f3b297.JPG)

![49ff](https://user-images.githubusercontent.com/79808404/233423945-e5c4da98-22ae-4cb8-871b-ec819d8adabb.JPG)

![49fff](https://user-images.githubusercontent.com/79808404/233423948-224e6eaf-7de6-49aa-9a93-cea31ba45ab8.JPG)

### _Create Launch Template For Wordpress_
![49ffff  create launch template for wordpress](https://user-images.githubusercontent.com/79808404/233423952-a9133e3b-c8c3-44b9-a458-f36d63503488.JPG)

![49ffff](https://user-images.githubusercontent.com/79808404/233423957-dd0f041f-dc4b-43ca-9f3f-e51c07f67a73.JPG)

![49fffff](https://user-images.githubusercontent.com/79808404/233423962-2f027757-b98c-4b7d-984b-f716a792132e.JPG)

![49fffff](https://user-images.githubusercontent.com/79808404/233423963-dee9f67d-8cca-4187-b1d8-ddc204610366.JPG)


### _Launch Template For Tooling_
![50  create launch template for tooling](https://user-images.githubusercontent.com/79808404/233423969-6dd3daa1-d9b7-44da-9251-faa4dd41d1f6.JPG)

![50a](https://user-images.githubusercontent.com/79808404/233423971-6f8fe827-1a49-45c4-8d7d-c665c9482506.JPG)

![50b](https://user-images.githubusercontent.com/79808404/233423974-673b3cfd-910f-46a0-a4e5-7baa03512402.JPG)

![50c  copying mount point for tooling](https://user-images.githubusercontent.com/79808404/233423975-2f256f54-6eb4-4b18-93e4-d112d5a517a0.JPG)

![50cc](https://user-images.githubusercontent.com/79808404/233423938-7a976968-ead8-4da4-bbf1-aff8a0d713aa.JPG)

![50ccc](https://user-images.githubusercontent.com/79808404/233423942-37687454-9dfa-4fa5-886e-d2435f5e5f6f.JPG)

![50cccc](https://user-images.githubusercontent.com/79808404/233424525-67b3bb3b-c2b4-48ae-aae2-445679791b57.JPG)

![50ccccc](https://user-images.githubusercontent.com/79808404/233424531-06fb20e1-d1e1-4899-8ff9-1071c8a41924.JPG)






















 
 
 
 

 
 
 
 
