# ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10

# <strong>Bastion or Jumper Server</strong> 

A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces [attack surface](https://en.wikipedia.org/wiki/Attack_surface)

![image](https://user-images.githubusercontent.com/29310552/163626519-961e9141-dc67-49d8-aad6-ff49bc7583fd.png)

## Task in completing this project

- Install and configure Ansible client to act as a Jump Server/Bastion Host
- Create a simple Ansible playbook to automate servers configuration

## STEP 1: INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

<img width="758" alt="1" src="https://user-images.githubusercontent.com/29310552/163630118-3cdc5f41-a46e-4e7e-b75e-e69f0fc38877.PNG">

In your GitHub account create a new repository and name it ansible-config-mgt.

<img width="681" alt="2" src="https://user-images.githubusercontent.com/29310552/163630138-8cf0ef0a-09aa-4acd-919a-830e035527c3.PNG">


Instal Ansible: In installing Ansible run the following command in your terminal:

`$ sudo apt update`

<img width="769" alt="3" src="https://user-images.githubusercontent.com/29310552/163630360-71986d9e-01d4-4c9e-bb33-f7c5d4f2453c.PNG">


`$ sudo apt install ansible`

<img width="849" alt="4" src="https://user-images.githubusercontent.com/29310552/163630375-ff75da42-b813-4102-b800-d7b6ec6715e0.PNG">



