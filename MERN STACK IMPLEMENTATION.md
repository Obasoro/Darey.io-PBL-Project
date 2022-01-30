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
 
```
const express = require('express');
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
}); 
```
 
<img width="500" alt="paste to do express" src="https://user-images.githubusercontent.com/29310552/151193974-7e757b58-4648-40d8-9327-f0ed5824a83c.PNG">

 
Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.
 
To save press *esc* and *:wq*
 
Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

`$ node index.js`
 
<img width="500" alt="Capture" src="https://user-images.githubusercontent.com/29310552/151195738-50dbd234-9ecc-4706-b7db-0a926d651440.PNG">

After setting the **Imbound** rules on the EC2 console
 
<img width="754" alt="imbound rule" src="https://user-images.githubusercontent.com/29310552/151195964-71edbc4c-2f63-4b48-a441-0d1f71987eba.PNG">
 
Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:
 
`http://<PublicIP-or-PublicDNS>:5000`

<img width="589" alt="ip5000" src="https://user-images.githubusercontent.com/29310552/151196372-1b763681-07d9-4d9f-98c8-5f4e98f4ab2f.PNG">
 
*Quick reminder how to get your server’s Public IP and public DNS name:*
1) You can find it in your AWS web console in EC2 details
*2) Run curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address or*
*3) Run curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name.*


# Routes
 
There are three actions that our To-Do application needs to be able to do:

- Create a new task
- Display list of all tasks
- Delete a completed task

If you are working on Mobaxterm, and the port 5000 is running, don't end the the process, click another session to continue:
 
<img width="681" alt="new session" src="https://user-images.githubusercontent.com/29310552/151203501-c94effe7-40be-4bc4-b610-7bec79f825e8.PNG">
 
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes
 
`mkdir routes`
 
*Tip*: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to routes folder.
 
`$ cd routes`
 
 Now we can create a file called api.js with the command below

 `touch api.js`
 
Open the file with the command below
 
<img width="505" alt="files-route-api" src="https://user-images.githubusercontent.com/29310552/151206753-231aa387-aa38-4d24-bca7-11517fc8dd47.PNG">

 
`$ vim api.js`
 
 
Paste the following code inside the vim editor
 
```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
 
```
<img width="450" alt="vim-route" src="https://user-images.githubusercontent.com/29310552/151206566-d6e43e20-1b5e-45f9-a368-5b93208a397b.PNG">

Moving forward let create Models directory.
 
# Model
 
Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document
 
To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with cd .. and install Mongoose
 
`$ npm install mongoose`
 
<img width="685" alt="mongoose -install" src="https://user-images.githubusercontent.com/29310552/151211007-c82bca2a-901d-4050-8b73-73ca2c73d3e4.PNG">
 
You can run the following command one after the other or run it concurrently
 
Create a new folder models :
 
`$ mkdir models`
 
`$ cd models`
 
`$ touch todo.js`
 
*Tip*: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:

`$ mkdir models && cd models && touch todo.js`
 
`$ vim todo.js`
 

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

 <img width="463" alt="vim todo" src="https://user-images.githubusercontent.com/29310552/151213865-9281cfd3-b044-42c2-9430-bdb190895aec.PNG">


 
Navigate to *api.js* location and open it through the vim editor

Delete the code within using the :%d within the vim editor
 
Copy and paste this code within the vim editor
 
```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
 
```
 
`<img width="538" alt="api vim" src="https://user-images.githubusercontent.com/29310552/151213703-e958e7b0-eb8f-43aa-a293-6e289639136b.PNG">
 

# Step 4: MONGODB DATABASE
 
*MongoDB Database*
 
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution
 
Log into the [mongodb](https://www.mongodb.com/cloud/atlas/lp/try2?utm_source=google&utm_campaign=gs_emea_nigeria_search_core_brand_atlas_desktop&utm_term=mongodb%20atlas%20login&utm_medium=cpc_paid_search&utm_ad=e&utm_ad_campaign_id=12212624539&adgroup=115749718503&gclid=Cj0KCQiA6NOPBhCPARIsAHAy2zCuSVQjuYblXZcppC6y8rBPRLZYT9bM9yIJCGRuQRDTniWRWbb4Q6oaAsuFEALw_wcB) and create a free account.
 
Create a database and a cluster<img width="816" alt="Monggo-Db" src="https://user-images.githubusercontent.com/29310552/151682205-07c554bb-ee87-480b-8543-f3d91d28c0c6.PNG">
 
<img width="582" alt="connect-db" src="https://user-images.githubusercontent.com/29310552/151682209-d5e1ddf4-8cc6-4c73-b572-5aad601f5106.PNG">
 
Locate the directory `Todo` and create a `.env` file
 
`$ touch .env`

`$ vi .env`
 
<img width="502" alt="vim-env" src="https://user-images.githubusercontent.com/29310552/151682266-0255b145-739c-4ebb-b9c5-48429a72610c.PNG">
 
Paste the following code into vim editor

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`
 
Replace username with the username on mongo console, also the database name with dname, and password when creating the cluster
 
We need to update the index.js to reflect the .env file we just created so we can connect with our database
 
Open index.js file using vim or nano as you might prefer. Delete the existing file and copy and paste 
 
```
 const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

```
Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file
 
Start the server by running this code
 
`$ node index.js`
 
<img width="499" alt="database connect" src="https://user-images.githubusercontent.com/29310552/151684203-20c0e85c-2990-4f4f-9e54-87ed37b1b4c4.PNG">
 
### Testing Backend Code without Frontend using RESTful API
 
- Download Postman
- Install and initiate the process
- Follow the instruction on this link [Postmant](https://www.youtube.com/watch?v=FjgYtQK_zLE)
 
### Get and Post Request Using Postman
 
<img width="670" alt="post-req" src="https://user-images.githubusercontent.com/29310552/151684896-256da998-b6aa-4037-bd2b-a0f7dcb10b2d.PNG">
 
<img width="671" alt="get-pos" src="https://user-images.githubusercontent.com/29310552/151684903-eca0a919-6d79-47e5-9451-d952c72d5555.PNG">



 
 


 


