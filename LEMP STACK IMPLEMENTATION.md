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

 






 
