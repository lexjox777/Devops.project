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

![29  config bastion](https://user-images.githubusercontent.com/79808404/233380400-b7e5767c-f183-4ac4-8cc9-57c27a2316b7.JPG)

![29b  config bastion (python install)](https://user-images.githubusercontent.com/79808404/233380433-3b07b6d7-45e2-4a24-8cd2-f6cabb79abbd.JPG)

![29c  config bastion](https://user-images.githubusercontent.com/79808404/233380449-fff3dc89-df20-4039-9ff3-1035b1c0f4f0.JPG)

![30  config nginx ami](https://user-images.githubusercontent.com/79808404/233380477-7432f7fc-9613-4730-b64d-1f5707530bb4.JPG)

![30b  config nginx ami (python install)](https://user-images.githubusercontent.com/79808404/233380504-3e677bd3-d2b1-4cbe-9333-ea55cda52927.JPG)

![30c config nginx ami(start and enable)](https://user-images.githubusercontent.com/79808404/233380531-0a76adfe-9d8d-46d1-b743-d44f29347f0a.JPG)

![30d  config nginx ami (set policies)](https://user-images.githubusercontent.com/79808404/233380581-d0590331-a35a-4123-bece-4f36a561ec33.JPG)

![31  install amazon efs utils](https://user-images.githubusercontent.com/79808404/233380613-a31a1ed0-a996-400b-ba03-c526e440108f.JPG)

![32  install self signed cert](https://user-images.githubusercontent.com/79808404/233380656-9b59b417-1f45-4e4a-bdd3-bb1e47963b1d.JPG)


![32b  install self signed cert](https://user-images.githubusercontent.com/79808404/233380689-fe1d8479-b96a-4a75-9275-d1d85d29f9a1.JPG)


![32c  install self signed cert](https://user-images.githubusercontent.com/79808404/233380736-018ffb89-6852-43c9-9097-74e7a0de724c.JPG)

![33  config webserver ami](https://user-images.githubusercontent.com/79808404/233380776-0f3f8679-5780-4d01-9039-c685f54ac410.JPG)


![33b  config webserver ami](https://user-images.githubusercontent.com/79808404/233380809-edeb62f9-5b03-43cd-9e75-5b5aa57cc6b1.JPG)


![33c  config webserver ami (enable and start )](https://user-images.githubusercontent.com/79808404/233380880-55f6ccc5-ceb4-4e3c-81b7-46c0158dc523.JPG)


![34  config webserver( selinux policies)](https://user-images.githubusercontent.com/79808404/233380917-d96d5f99-1989-4938-8270-75a6d75f6c3e.JPG)


![35  config webserver (install amazon efs utils)](https://user-images.githubusercontent.com/79808404/233380939-d002fd95-0b94-4858-8a3a-df8db94d83b9.JPG)

![36  install self signed cert for apache webserver](https://user-images.githubusercontent.com/79808404/233380971-b86bc33b-a571-4231-a26c-e395e289f063.JPG)

 
 ![36b  install self signed cert](https://user-images.githubusercontent.com/79808404/233381044-11e88aaf-2c91-48af-be66-0460d6ff7f80.JPG)

 
 ![36c  edit localhost to acs crt](https://user-images.githubusercontent.com/79808404/233381081-f6787561-8589-49e1-88f2-9aaf9ec5223f.JPG)

 ![36c](https://user-images.githubusercontent.com/79808404/233381119-d411767c-6b00-4c98-8946-fa39f1c8bb72.JPG)

 
 
 
 
