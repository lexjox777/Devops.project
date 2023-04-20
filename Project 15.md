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


# Step 4. I created subnets (private and public subnets) This allows better organization and well management of my resouces, improved network security(by distinguishing between a public subnet such as webserver and a subnet that requires a private access like databases) and also improve network availabilty by spreading across multiple Availabilty Zones.


![6  create subnets](https://user-images.githubusercontent.com/79808404/233322643-5f546a42-d1d3-494b-ac56-b1ecafc2c7b4.JPG)


![6b  public subnet 1 created](https://user-images.githubusercontent.com/79808404/233322689-940f0344-b310-48b9-89eb-cc1d40bfaeb9.JPG)


![6c  public subnet 2 created](https://user-images.githubusercontent.com/79808404/233322708-f1ceebd5-62de-49af-bdf7-215ff1fa0163.JPG)


![7  private subnet 1 created](https://user-images.githubusercontent.com/79808404/233322823-2c971476-014a-47a6-a523-99fd049a0ac0.JPG)


![7b  private subnet 2 3 created](https://user-images.githubusercontent.com/79808404/233322981-42ff4a9b-f3a9-4df5-b9f0-7e2209657404.JPG)

![7c  private subnet 4 created](https://user-images.githubusercontent.com/79808404/233323010-cd01566d-9496-4bbf-8fb2-138cae459b62.JPG)



# Step 5. I created Route tables( Private and Public route tables) and associated it with my subnets

  
![8  created route table](https://user-images.githubusercontent.com/79808404/233325533-d2c64df8-b500-4797-b9b4-21283dfdd67e.JPG)


![8b  public route table created](https://user-images.githubusercontent.com/79808404/233325570-4f4bf262-7711-45b3-8bd6-32278dacd7e8.JPG)

![8c  private route table created](https://user-images.githubusercontent.com/79808404/233325813-20908fbb-6840-4049-bd93-972e913d743c.JPG)


![9  associate public rtb to public subnet](https://user-images.githubusercontent.com/79808404/233326005-788c9679-08b1-4962-8e61-5674121f0545.JPG)


![9b](https://user-images.githubusercontent.com/79808404/233326072-ce8f99c7-088b-47bf-9fbc-d0d4d68e43e2.JPG)


![10  associate private rtb to private subnet](https://user-images.githubusercontent.com/79808404/233326123-e7850293-ab74-4e03-8375-11638e96b57d.JPG)


![10b](https://user-images.githubusercontent.com/79808404/233326150-7c744274-5ed8-4dd3-9038-a6aff6963ce1.JPG)


# Step 6. I edited the private and public route table to to target my IGW


![11  edit public rtb](https://user-images.githubusercontent.com/79808404/233328999-cf24012d-8ede-4859-ad9e-eb0d96d5dcc4.JPG)

![11b](https://user-images.githubusercontent.com/79808404/233329033-3422ad62-02ee-492f-9d39-967e7c91bf33.JPG)


# Step 7. I created and allocated an Elastic IP for NAT Gateway within my VPC to allow instances in Private Subnets to access the internet without exposing their IP addresses and still keeping them protected from incoming traffic from the internet.


![12  allocate elastic ip for NAT gateway](https://user-images.githubusercontent.com/79808404/233337956-221cb642-0b66-40ae-a5b2-181011cfbc90.JPG)


![12b](https://user-images.githubusercontent.com/79808404/233337993-f478e9a4-0bbd-4c55-aa62-39ec8e23cbaa.JPG)


![13  Create a NAT Gateway](https://user-images.githubusercontent.com/79808404/233338011-83f9b31d-7d1e-4159-a5d4-531c7ca4d401.JPG)



![13b](https://user-images.githubusercontent.com/79808404/233338080-7ad5e025-4dcd-45a4-8d71-9fc4dcc1a439.JPG)












 
 
 
 
 
 
 
 
 
