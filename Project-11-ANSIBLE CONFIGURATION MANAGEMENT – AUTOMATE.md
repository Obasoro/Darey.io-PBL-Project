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

To check out the <bold>Ansible</bold> version, run this command:

`$ ansible --version

<img width="826" alt="4 1" src="https://user-images.githubusercontent.com/29310552/163631350-854e97d5-a222-43b2-aca0-c736bef74191.PNG">

Configure Jenkins build job to save your repository content every time you change it 

![image](https://user-images.githubusercontent.com/29310552/163634371-685b9774-3d97-4449-a6c7-f35bdeec5701.png)

   Configure github repository for webhooking
  - Click settings on the github
  - click webhook
  - input the <jenkins server ip>/github-webhook/


  - Copy the IP-address:8080 to initialize jenkins.
  - configure jenkins by selecting the New Item
  - Click Freestyle (depending on what you are doing)
  - click git as source code management 
  - change the master to main
  - Click github hook triggers for Triggers
  - for the post build, choose archive artifact fill in **
  - click build now
  
  
<img width="741" alt="jenk1" src="https://user-images.githubusercontent.com/29310552/163634895-c3282614-8dc5-47f8-992c-06bd9e667f83.PNG">
  
  
<img width="853" alt="jwn3" src="https://user-images.githubusercontent.com/29310552/163634903-a36a349b-ee14-4adc-8a91-b7938929888b.PNG">
  
  
<img width="703" alt="jenks2" src="https://user-images.githubusercontent.com/29310552/163634898-395f1415-3e2c-46fb-af85-abd7b135f8fd.PNG">
  
  
<img width="762" alt="jenks4" src="https://user-images.githubusercontent.com/29310552/163634901-6b83a505-2fcb-4c85-adc9-2198972a2b56.PNG">
  


Test the setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder
  
Run this command
  
`$ sudo ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`
  
<img width="588" alt="4 2" src="https://user-images.githubusercontent.com/29310552/163635763-64f52042-2cfb-4684-8244-77357dee38e2.PNG">
  
Now your setup will look like this:
  
![image](https://user-images.githubusercontent.com/29310552/163635845-deb84dfd-cf76-47d2-8ba5-5b2be1209321.png)
   

   
## Step 2 – Prepare your development environment using Visual Studio Code
   
First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs.
   
VScode allow for proper development environment. You can downlaod [vscode](https://code.visualstudio.com/download).
   
Install several needed extension that would allow us to work conveniently on vscode
   
 - Remote development server
 - git (enable git)
 - create a folder and clone the git repository into it.
   
 <img width="674" alt="5" src="https://user-images.githubusercontent.com/29310552/163650421-c16432bf-2502-4b5b-b808-36e5e072a894.PNG">


   
# BEGIN ANSIBLE DEVELOPMENT
   
Check out the git status after cloning.
   
<img width="614" alt="1 o" src="https://user-images.githubusercontent.com/29310552/163651098-c2daa10d-2ce1-42e7-b88a-0ff305f1d41e.PNG">


<img width="762" alt="5 10" src="https://user-images.githubusercontent.com/29310552/163650440-a65110dd-9044-4fdb-ae6b-6a10004744f8.PNG">
   
Checkout the newly created feature branch to your local machine and start building your code and directory structure
   
`$ git checkout -b <project-title>
   
<img width="604" alt="1 1" src="https://user-images.githubusercontent.com/29310552/163651302-3067551b-a455-4d3a-9f2c-7ce1b09c5f77.PNG">
   

Create a directory and name it playbooks – it will be used to store all your playbook files
   
`$ mkdir playbooks`
   
Create a directory and name it inventory – it will be used to keep your hosts organised.
   
`$ mkdir inventory`
   
Within the playbooks folder, create your first playbook, and name it common.yml
   
`$ cd playbooks`
   
`$ touch common.yml`
   
  
Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.
   
`$ cd inventory`
   
`$ touch dev.yml staging.yml uat.yml prod.yml
   
<img width="618" alt="1 3" src="https://user-images.githubusercontent.com/29310552/163651921-3030befa-ef40-45af-b365-da4154404719.PNG">
   
# Step 4 – Set up an Ansible Inventory
   
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.





  

  





