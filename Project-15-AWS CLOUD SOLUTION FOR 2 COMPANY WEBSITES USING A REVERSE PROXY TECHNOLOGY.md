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
 ![image](https://user-images.githubusercontent.com/29310552/179436112-9450ef92-a651-43e9-8f89-f80fbe834318.png)





 

  
 
