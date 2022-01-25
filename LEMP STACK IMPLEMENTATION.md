# PROJECT 2: LEMP STACK IMPLEMENTATION
***
### Step 0: Using PuttyGen App and Git Bash to access to VM on AWS
***
- #### Using PuttyGen App in connecting to EC2 ubuntu VM
***
  - Download PuttyGen App and run the Application
  - Load the private key to convert the from .pem format to .ppk format
  - ![putty](https://user-images.githubusercontent.com/29310552/150975531-74c8afc2-7aec-49e4-ade6-014c80240a4f.JPG)
  - Load the Putt App and paste the DNS-IP of EC2 on the host 
  - ![PUty](https://user-images.githubusercontent.com/29310552/150975589-c7b508cd-8457-482b-a09a-3921385560d3.JPG)
  - A login page would be displayed, for the login sign use ubuntu. 
  - ![Putty-Ubuntu](https://user-images.githubusercontent.com/29310552/150976988-c439ec43-0c4f-4878-b973-7404477e9f01.JPG)
  - This Would connect to the VM on AWS
  - ![Putty-Ubuntu1](https://user-images.githubusercontent.com/29310552/150977095-59931ade-de17-4400-b086-872d8b61e9cb.JPG)
***
- #### Using Gitbash to connect EC2 Instance on AW
***
  - Download Gitbash and install accordingly
  - cd into the directory where the private key downloaded was stored.
  - copy the SSH command and paste
  - ![VM on gtbas](https://user-images.githubusercontent.com/29310552/150978457-c06a6815-b4f3-41e2-8728-96d8ff8ac347.JPG)

# Step 1: INSTALLING THE NGINX WEB SERVER

 In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web  server. We’ll use the apt package manager to install this package.

 We would start by updating the created ubuntu instance on AWS using the following command line
 
 `$ sudo apt update`
 
 <img width="531" alt="Sudo-apt-update" src="https://user-images.githubusercontent.com/29310552/150980851-12b35cb7-3989-4c43-8f99-d1b7158e81ac.PNG">
 
 We would now install the nginx web server using this command
 
 `$ sudo apt install nginx`
  
 We would comfirm if the nginx server is now running and active by running this command
 
 `$ sudo systemctl status nginx`
 
 <img width="531" alt="sudo-system-nginx" src="https://user-images.githubusercontent.com/29310552/150981305-8a6f6fc1-7337-4190-b3e5-b1a8bafe78c1.PNG">
 
 We can check the webserver is up and running by running these two command. The would give us same result.
 
`$ curl http://localhost:80`
<img width="534" alt="localhost" src="https://user-images.githubusercontent.com/29310552/150982720-967e039f-681a-4de7-a3bb-bcf21af22c52.PNG">

or

`$ curl http://127.0.0.1:80`

<img width="528" alt="port80" src="https://user-images.githubusercontent.com/29310552/150982783-fb7bacb8-5fc2-40ff-98ee-980777104240.PNG">

Now it is time for us to test how our Nginx server can respond to requests from the Internet.
Open a web browser of your choice and try to access following url

`http://<Public-IP-Address>:80`

<img width="711" alt="nginxIP-80" src="https://user-images.githubusercontent.com/29310552/150983197-7af5a3b8-aad3-41b0-9ada-ae7ce0f2c9b5.PNG">

Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`


# Step 2: INSTALLING MYSQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site

`$ sudo apt instal mysql-server`

<img width="675" alt="mysql-sever" src="https://user-images.githubusercontent.com/29310552/150987377-113d116e-4156-458d-b50b-d87e63faaffd.PNG">

When prompted, confirm installation by typing Y, and then ENTER.

After completion of the installation

To secure your mysql, we would need to run the following command

`sudo mysql_secure_installation`

<img width="674" alt="msql -yes-1" src="https://user-images.githubusercontent.com/29310552/150987463-35040d4b-9a60-4d97-aa24-2efbabb55ece.PNG">

This would prompt different question on security and passwords. 

`**Note:** Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.`

`VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?`

<img width="673" alt="msql -yes-2" src="https://user-images.githubusercontent.com/29310552/150987527-f119756e-f601-4985-82c2-690aa4067801.PNG">

<img width="673" alt="3" src="https://user-images.githubusercontent.com/29310552/150987551-272a88ec-3ac3-42b7-b44b-87e20a73f870.PNG">


Press y|Y for Yes, any other key for No:


Type y (Yes) for all and also Enter

We can now install mysql by running this command

`$ sudo mysql`

<img width="635" alt="sudo-mysql" src="https://user-images.githubusercontent.com/29310552/150989110-d4d6c4ef-540d-4dac-bc0d-b1c8ac2bc6e2.PNG">

Our MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack

# Step 4: INSTALLING PHP

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration.

Two libraries would installed simultaneously using this command

`$ sudo apt install php-fpm php-mysql`

<img width="672" alt="php load" src="https://user-images.githubusercontent.com/29310552/150990111-e7f909ed-e720-4d6e-93d2-11bf21ad431a.PNG">

A Yes prompt would be show within the script, type y to complete the process of installation

# Step 4: CONFIGURING NGINX TO USE PHP PROCESSOR

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

To create a root web directory to the domain site, run the code below 

`$ sudo mkdir /var/www/projectLEMP`

We need to assign $USER ownrship within the $USER environmentt. With this we can reference the current system user

`$ sudo chown -R $USER:$USER /var/www/projectLEMP`


We are going to use NANO editor to open a new configuration file using this command

`$ sudo nano /etc/nginx/sites-available/projectLEMP`


<img width="480" alt="nano-edit" src="https://user-images.githubusercontent.com/29310552/151001662-1e503541-e0b2-42ee-a330-9e6f46830c7e.PNG">

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

 

 






 
