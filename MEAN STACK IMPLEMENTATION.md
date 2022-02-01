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

To create a directory Books and cd into the directoty to run this command

`$ mkdir Books && cd Books`

`npm init`

<img width="478" alt="bookswork" src="https://user-images.githubusercontent.com/29310552/151895279-b4f30afe-5453-4994-81c6-c890e50c93c1.PNG">

We would now add a file server.js

`$ vi server`

Copy and paste this code below

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

```


# Install Express and Set Up Route to the Server

# Step 3: Install Express and set up routes to the server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.

We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

`$ sudo npm install express mongoose`
<img width="484" alt="npm-mongos" src="https://user-images.githubusercontent.com/29310552/151896690-b4452d35-5ada-4fd0-9746-34311f673952.PNG">

In the Book folder, create a folder named apps

Create a file routes.js and paste this code below

`$ vi routes.js`

```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};

```

In the ‘apps’ folder, create a folder named models

Create a file book.js and paste this code below

`$ vi book.js`

<img width="384" alt="book" src="https://user-images.githubusercontent.com/29310552/151900231-55ff353f-8434-44f2-a2c1-52b979c9b4a1.PNG">

# Step 4 – Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’

`$ cd ../..`

Create a folder publc and a file inside script.js

`$ mkdir public && cd public`

`$ vi script.js`

<img width="721" alt="scrit" src="https://user-images.githubusercontent.com/29310552/151901010-f143b527-22d1-4739-960d-35013e7ebe27.PNG">

In public folder, create a file named index.html;

Change the directory back up to Books

`$ cd ..`

Start the server by running this command:

`$ node server.js`

<img width="465" alt="server" src="https://user-images.githubusercontent.com/29310552/151901567-b3e31151-14df-4de1-8ef1-c898f185d404.PNG">

The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

`$ curl -s http://localhost:3300`

<img width="808" alt="curls" src="https://user-images.githubusercontent.com/29310552/151902028-0049ba44-9c8c-493a-923b-f90a2fd46d04.PNG">

Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

<img width="326" alt="complete" src="https://user-images.githubusercontent.com/29310552/151902540-0b888333-bb63-463b-a9ed-74971e158fe1.PNG">

Step are from [Darey.io](https://www.darey.io/)


 

   
 
