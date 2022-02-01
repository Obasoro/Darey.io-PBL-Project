# Step 0 – Preparing prerequisites

In order to complete this task, we must create an instance on EC2. You can check the steps on the previous framework deployed.

<img width="709" alt="mean1" src="https://user-images.githubusercontent.com/29310552/151805678-270db6a1-5030-4ac8-b4b7-6a0ef63c77fc.PNG">


# Task 
In this assignment you are going to implement a simple Book Register web form using MEAN stack.

 ### Install Nodejs
  -1 We would update the Ubuntu by running command
   `$ sudo apt update`
   <img width="697" alt="mean2" src="https://user-images.githubusercontent.com/29310552/151812667-f2064a90-c7fc-42c6-b7e7-5e5bce8045a8.PNG">

  -2 We upgrade the ubuntu
   `$ sudo apt upgrade`
   <img width="765" alt="mean3" src="https://user-images.githubusercontent.com/29310552/151813025-64186e83-7da0-4a78-a4fd-ff11e42215ef.PNG">
  
  Add certificates
  `$ sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`
  
  <img width="480" alt="ca-cert" src="https://user-images.githubusercontent.com/29310552/151883486-10b7f466-2b7a-4554-94af-7eb68e56e43f.PNG">

  
  `$ curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
  
 ## Install Nodejs
  `$ sudo apt install -y nodejs`
  
 # Step 2: Install MongoDB
 
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

`$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

<img width="481" alt="steps-mongo" src="https://user-images.githubusercontent.com/29310552/151885963-84fa3bce-88cd-421e-ab86-90276080ea71.PNG">

 
`$ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

## Install MongoDB

`$ sudo apt install -y mongodb`

Start the Server

`$ sudo service mongodb start`

Verify that the service is up

`$ sudo systemctl status mongodb`

Install [npm](https://www.npmjs.com/) - Node package manager

`$ sudo apt install -y npm`

Install body-parser by running this command

`$ sudo npm install body-parser`
<img width="481" alt="parser" src="https://user-images.githubusercontent.com/29310552/151894204-18a56f95-c986-45da-976c-b036ed7c697c.PNG">


 

   
 
