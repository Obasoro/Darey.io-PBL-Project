# *This Project is about to install MERN STACK on AWS Cloud.*

## What is MERN STACK?
***

**MERN** stands for MongoDB, Express, React, Node, after the four key technologies that make up the stack.

- MongoDB - document database
- Express(.js) - Node.js web framework
- React(.js) - a client-side JavaScript framework
- Node(.js) - the premier JavaScript web server


*"MERN Stack is a Javascript Stack that is used for easier and faster deployment of full-stack web applications. MERN Stack comprises of 4 technologies namely: MongoDB, Express, React and Node.js. It is designed to make the development process smoother and easier.*

*Each of these 4 powerful technologies provides an end-to-end framework for the developers to work in and each of these technologies play a big part in the development of web applications."*

**culled from** [GeeksforGeeks](https://www.geeksforgeeks.org/mern-stack/)

For this **Project 3** we would be using the *Mobaxterm* to connect to our *Ubuntu* VM created on **AWS**

## Step 0: Preparing prerequisites

 - Long on on to your **AWS** account to create an instance on *EC2*
 - For the procedure to create an Instance click on [Darey.io](https://www.youtube.com/watch?v=xxKuB9kJoYM)
 - Save the downlaoded private key within a folder
For **Windows** users, apart from Window subsystem Linux (WSL), Gitbash and PuttyGen App, we could also use Mobaxterm App to connect.

Mobaxterm allows for multiple session with the CLI environment.

<img width="979" alt="Mobaxterm" src="https://user-images.githubusercontent.com/29310552/151159278-0783304d-ab65-4726-afa6-065b53e3f540.PNG">

 - # Using Mobaxterm to connect to EC2 Instance
  - Download the Mobaxterm and click the option you prfer, free or paid, depending on what you intend achieving
  - Click the app to initiate the process of using it
  - <img width="979" alt="Mobaxterm" src="https://user-images.githubusercontent.com/29310552/151161089-284c9ff1-222b-47d3-bcf6-880a1e85a100.PNG">
  - Click new session icon on the extreme left tab of the Application
  - Click SSH
  - Paste the IP address of EC2 at the remote name on as displayed and write the preferred name of your instance
  - Click the advance setting on the middle of the application
  - Click private key, select the location of the private key on your system and load.
  - Click ok
  - <img width="641" alt="ssh-moba" src="https://user-images.githubusercontent.com/29310552/151162823-9635fdfd-bbbd-4f03-88ba-ca9a2c61403d.PNG">
  - The Ubuntu system is upon running on your system 
  - <img width="680" alt="ubuntu-moba" src="https://user-images.githubusercontent.com/29310552/151162869-d5d3f76a-2d61-480e-b3c0-824af8255102.PNG">

<strong>TASK
 
To deploy a simple To-Do application that creates To-Do lists like this:

# Step 1: BACKEND CONFIGURATION
 
We would run these command on our ubuntu linux distribution

`$ sudo apt update`
 
 <img width="688" alt="update instance" src="https://user-images.githubusercontent.com/29310552/151164912-28e5474a-b87b-49c3-9f3d-6b9537b48f5a.PNG">


`$ sudo apt upgrade`
 
<img width="678" alt="upgrade" src="https://user-images.githubusercontent.com/29310552/151165303-c099cd5c-3bca-4502-92e7-fcf34124b18f.PNG">

To locate the position of the Node.js within the Ubuntu repositories
 
`$ curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
 
<img width="686" alt="curl-deb" src="https://user-images.githubusercontent.com/29310552/151166087-d061d967-4ea0-4bbe-bfa1-5662f79dc970.PNG">
 
By running this command below, we can install nodejs
 
`$ sudo apt-get install -y nodejs`
 
 <img width="686" alt="nodejs-moba" src="https://user-images.githubusercontent.com/29310552/151167058-b184fd8e-240e-463e-987a-f4c641284efb.PNG">

 
**Note**: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts
 
To check the version sinstallation

`$ node -v`
 
Verify the node installation with the command below

`$ npm -v`
 
 <img width="482" alt="npm-v" src="https://user-images.githubusercontent.com/29310552/151194071-4583222b-98c5-4a2c-a065-9fcd0dcca090.PNG">

 

 
**Application Code Setup**
 
Let us create a new directory for our TO-Do task

`$ mkdir Todo`
 
Run the command below to verify that the Todo directory is created with ls command
 
`$ ls`
 
**TIP**: In order to see some more useful information about files and directories, you can use following combination of keys ls -lih – it will show you different properties and size in human readable format.
 
Now change your current directory to the newly created one:
 
`$cd Todo`
 
Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes
 
`$ npm init`
 
<img width="501" alt="mobax-npm-init" src="https://user-images.githubusercontent.com/29310552/151169731-4d9f57e2-12c7-476b-af62-b6a5e20458a9.PNG">
 

Next, we will Install ExpressJs and create the Routes directory.

# INSTALL EXPRESSJS

Install ExpressJS
Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:
 
`$ npm install express`
 
<img width="486" alt="npm-installa" src="https://user-images.githubusercontent.com/29310552/151193257-4e201387-a922-461e-8e82-7d50c320a580.PNG">

Now create a file index.js with the command below
 
`$ touch index.js`

 
Run the ls command and Install the dotenv module

`$ npm install dotenv`
 
<img width="500" alt="dotenv" src="https://user-images.githubusercontent.com/29310552/151193575-28b01404-a755-4a89-b012-4f3e21372e34.PNG">
 
Open the index.js file with the command below and paste the following code:
 
`const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});`
 
<img width="500" alt="paste to do express" src="https://user-images.githubusercontent.com/29310552/151193974-7e757b58-4648-40d8-9327-f0ed5824a83c.PNG">



 
 
 




  
  - 




