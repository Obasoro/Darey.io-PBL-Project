# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY

In this task we build a secure  infrastructure inside AWS VPC (Virtual Private Cloud) network for a fictitious company (Choose an interesting name for it) that uses WordPress CMS for its main business website, and a Tooling Website (https://github.com/<your-name>/tooling) for their DevOps team.
As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

![image](https://user-images.githubusercontent.com/29310552/179272004-c0c0e545-9f5f-4e76-8c39-69b5f7eb32a4.png)
 
# SET UP A VIRTUAL PRIVATE NETWORK (VPC)
## Set Up a Virtual Private Network (VPC)
 
[1]. Create VPC

![image](https://user-images.githubusercontent.com/29310552/179430837-cfd01864-623e-4031-a779-408775f835ba.png)
 
[2] Create subnets as shown in the architecture. You can use [ipinfo.io/ips](https://ipinfo.io)
 
![image](https://user-images.githubusercontent.com/29310552/179432062-b9ae3e70-f88c-458b-8c9c-7b37a9cbfadd.png)

![image](https://user-images.githubusercontent.com/29310552/179432291-d4b9102d-ccb2-4535-8346-b252a3041f0e.png)

[3] Create a route table and associate it with public subnets
 
![image](https://user-images.githubusercontent.com/29310552/179434123-9c1246fe-8152-499a-8c88-43e71d08f194.png)

[4] Create a route table and associate it with private subnets
 
![image](https://user-images.githubusercontent.com/29310552/179434327-7f22b703-1b76-4785-a071-aaf63e9f0736.png)

[5] Create an Internet gateway and attach it to the created VPC
 
![image](https://user-images.githubusercontent.com/29310552/179430985-dd9303ae-926d-4421-8d78-752f26d7c945.png)
 
[6] Edit a route in public route table, and associate it with the Internet Gateway. (This is what allows a public subnet to be accisble from the Internet)
 
![image](https://user-images.githubusercontent.com/29310552/179434482-b43f1d21-ba5b-430c-944f-97aaf6209299.png)
 
[7] Create 3 Elastic IPs
 
![image](https://user-images.githubusercontent.com/29310552/179435024-c11fcdaf-1445-4bd7-8f64-68d8424a61c2.png)

[8] Create a Nat Gateway and assign one of the Elastic IPs (*The other 2 will be used by Bastion hosts) 
 
![image](https://user-images.githubusercontent.com/29310552/179435205-cc8a5570-d767-4f5f-888c-70bbd3993bb0.png)

[9] Create a Security Group for:
- Nginx Servers: Access to Nginx should only be allowed from a Application Load balancer (ALB). At this point, we have not created a load balancer, therefore we       will update the rules later. For now, just create it and put some dummy records as a place holder.
 
 ![image](https://user-images.githubusercontent.com/29310552/179436112-9450ef92-a651-43e9-8f89-f80fbe834318.png)
 
- Bastion Servers: Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your           workstation public IP address. To get this information, simply go to your terminal and type curl www.canhazip.com
![image](https://user-images.githubusercontent.com/29310552/179436644-cb3b6b0a-0736-4445-bd04-32267335869c.png)
 
- Application Load Balancer: ALB will be available from the Internet
 
![image](https://user-images.githubusercontent.com/29310552/179437478-339d6fad-996b-4478-ada0-af30bd74fe3a.png)
 
![image](https://user-images.githubusercontent.com/29310552/179438053-ce4e0d60-e83f-402e-a794-7c4b7dd1fe08.png)

 
- Webservers: Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a       place holder, we will update it later.
![image](https://user-images.githubusercontent.com/29310552/179438667-0b50a5b3-8860-403f-b504-bed7b3cdc637.png)

- Data Layer: Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully         desinged – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.
 
![image](https://user-images.githubusercontent.com/29310552/179439442-4e1a625f-9578-4241-8d46-771208408671.png)
 
 
# Creating Computing Resources
 
![image](https://user-images.githubusercontent.com/29310552/179490426-a6776c86-9115-4078-9303-d2993b2a5d3a.png) 
 
 Below is the file <strong>EFS</strong> 
 
![image](https://user-images.githubusercontent.com/29310552/179493477-aa06efba-e39c-4fac-8f66-53e28efb8d0f.png)

Access point 
![image](https://user-images.githubusercontent.com/29310552/179494163-ba8118c8-8b40-4758-94af-83356ff2cd6e.png)
 
![image](https://user-images.githubusercontent.com/29310552/179494856-3d035a6e-a713-4ee0-9a75-2553b6452dc0.png)

Create a Key Management service account which generate a key

![image](https://user-images.githubusercontent.com/29310552/179496134-4741ac6c-6347-4b0b-bc3a-d7f65c007550.png)
 
Create a subnet group with all the availabilty and subnet group

![image](https://user-images.githubusercontent.com/29310552/179500175-a6c86118-e890-4ce0-acdf-8bbb742d40cf.png)

Create RDS

![image](https://user-images.githubusercontent.com/29310552/179501669-153a549b-94ec-4f0e-b022-cd7c838f3235.png)
 
## Set Up Compute Resources for Nginx
1. Create an EC2 Instance based on CentOS Amazon Machine Image (AMI) in any 2 Availability Zones (AZ) in any AWS Region (it is recommended to use the Region that is    closest to your customers). Use EC2 instance of T2 family (e.g. t2.micro or similar)
2. Ensure that it has the following software installed:

    - python
    - ntp
    - net-tools
    - vim
    - wget
    - telnet
    - epel-release
    - htop

![image](https://user-images.githubusercontent.com/29310552/179506422-5f6c8485-dab0-4b09-a48d-f6aa0401bd86.png)
 
In creating ami and install necessary modules [ami] (https://github.com/obasoro/ACS-project-config/blob/main/Installation.md)

![image](https://user-images.githubusercontent.com/29310552/179515419-2b69854f-c09a-46fb-a118-a27f5082ca85.png)
 
Create Application Load Balance template
 
Nginx EC2 Instances will have configurations that accepts incoming traffic only from Load Balancers. No request should go directly to Nginx servers. With this kind of setup, we will benefit from intelligent routing of requests from the ALB to Nginx servers across the 2 Availability Zones. We will also be able to offload SSL/TLS certificates on the ALB instead of Nginx. Therefore, Nginx will be able to perform faster since it will not require extra compute resources to valifate certificates for every request.

Create an Internet facing ALB
Ensure that it listens on HTTPS protocol (TCP port 443)
Ensure the ALB is created within the appropriate VPC | AZ | Subnets
Choose the Certificate from ACM
Select Security Group
Select Nginx Instances as the target group
![image](https://user-images.githubusercontent.com/29310552/179637786-2d0841c1-a773-446a-b2e5-d8d5ddaa63c4.png)
 
## Application Load Balancer To Route Traffic To Web Servers
Since the webservers are configured for auto-scaling, there is going to be a problem if servers get dynamically scalled out or in. Nginx will not know about the new IP addresses, or the ones that get removed. Hence, Nginx will not know where to direct the traffic.

To solve this problem, we must use a load balancer. But this time, it will be an internal load balancer. Not Internet facing since the webservers are within a private subnet, and we do not want direct access to them.

Create an Internal ALB
Ensure that it listens on HTTPS protocol (TCP port 443)
Ensure the ALB is created within the appropriate VPC | AZ | Subnets
Choose the Certificate from ACM
Select Security Group
Select webserver Instances as the target group
Ensure that health check passes for the target group
 
## Setup EFS
Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic Network File System (NFS) for use with AWS Cloud services and on-premises resources. In this project, we will utulize EFS service and mount filesystems on both Nginx and Webservers to store data.

Create an EFS filesystem
Create an EFS mount target per AZ in the VPC, associate it with both subnets dedicated for data layer
Associate the Security groups created earlier for data layer.
Create an EFS access point. (Give it a name and leave all other settings as default)
Setup RDS
Pre-requisite: Create a KMS key from Key Management Service (KMS) to be used to encrypt the database instance.
 
Create an Auto-scaling group
 

Accesing the RDS from the Bastion sever

![image](https://user-images.githubusercontent.com/29310552/179651876-48106ded-ab42-4845-ad82-64cf1a7e338c.png)
 
Using your tooling app
 
![image](https://user-images.githubusercontent.com/29310552/179732955-5b9ca880-06b3-448e-a908-770f98b4b661.png)
 
Health of the target group
![image](https://user-images.githubusercontent.com/29310552/179733225-2a2d6d4c-fa7f-4a13-89b3-4877e03805e3.png)

![image](https://user-images.githubusercontent.com/29310552/179733298-6cf49172-1c32-4962-b131-370f6b6a6da4.png)

![image](https://user-images.githubusercontent.com/29310552/179733370-2e44a1e4-c948-461a-b143-d31b525f54a7.png)
 
 
[darey.io](https://www.darey.io/docs/aws-cloud-solution-for-2-company-websites-using-a-reverse-proxy-technology/)

 














 

  
 
