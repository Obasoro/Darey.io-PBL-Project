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












 

  
 
