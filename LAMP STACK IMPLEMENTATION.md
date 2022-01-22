# PROJECT 1 - LAMP STACK IMPLEMENTATION

## Steps involved in implementation
---------------------------------------

### Step 1: Installing apache and updating the firewall
__________________________________________________________

update a list of packages in package manager

#### $ sudo apt update


![Apache1](https://user-images.githubusercontent.com/29310552/150621426-6461bd4a-80f9-441b-b5d3-46c60310814d.JPG)

run apache2 package installation

#### $ sudo apt install apache2

![Apach2](https://user-images.githubusercontent.com/29310552/150621532-925c2323-8209-465a-81d7-144686769317.JPG)

run the following command 

$ sudo systemctl status apache2
![Apache3runn](https://user-images.githubusercontent.com/29310552/150621618-c4bf58f6-1044-4f78-a7c4-34329ed7f9f9.JPG)

On the AWS console the imbound rules was adjusted to *Anywhere* to receive traffic
![Imbound Rules](https://user-images.githubusercontent.com/29310552/150621738-75ddceba-9d5e-48c4-b70c-ed282d2c0489.JPG)

#To run the apache2 locally use the command below:

$ curl http://localhost:80

or

$ curl http://127.0.0.1:80

To test the Apache server response on the internet, run this code below. An emphemeral IP was deployed for testing was

http://Public-IP-Address:80

or 

use this below: The both achieve same result.

$ curl -s http://169.254.169.254/latest/meta-data/public-ipv4

![Apache-install](https://user-images.githubusercontent.com/29310552/150622412-0af38000-1366-45aa-b495-4b7770cff48d.JPG)


### Step 2: Installing Mysql
***

#To install on linux system, need to run the apt command. So for this also we would run the following snippet of code

$ sudo apt install mysql-server
This validated by inputing a system of password from low to high depending on our choice

![Pass-word valaidation process 3](https://user-images.githubusercontent.com/29310552/150622912-98057aba-e7c8-439c-a20f-c269251298dd.JPG)

$ sudo mysql_secure_installation

![mysql-nstallation](https://user-images.githubusercontent.com/29310552/150622763-e68b6c6a-e06d-40e4-b5de-75bf9735aa9c.JPG)


