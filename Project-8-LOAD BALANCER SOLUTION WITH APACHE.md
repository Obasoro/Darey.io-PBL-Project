# LOAD BALANCER SOLUTION WITH APACHE

When we access the internet using <strong>url</strong> we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).

Each URL contains a domain name part, which is translated (resolved) to IP address of a target server that will serve requests when open a website in the Internet. Translation (resolution) of domain names is perormed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with nslookup command.

The `nslookup` installation on RHEL/CentOs. We must first access it at root user using the `sudo su`

`$ sudo su`

<img width="282" alt="Capture" src="https://user-images.githubusercontent.com/29310552/161868438-054ee781-3fed-47a2-a793-e53b21c9882b.PNG">

Run this command below

`# dnf install bind-utils`

<img width="820" alt="1" src="https://user-images.githubusercontent.com/29310552/161868608-02d577da-45b1-4524-ae35-aceaacfa8443.PNG">

Run the command below to verify installation

`# dig -v`

<img width="365" alt="2" src="https://user-images.githubusercontent.com/29310552/161868756-c22c4ba3-84a1-4f9c-95f8-f0b2a2ca6ad5.PNG">

`nslookup 8.8.8.8`

<img width="364" alt="3" src="https://user-images.githubusercontent.com/29310552/161868852-ad25e9ba-cbe4-41cc-9806-4793f82a7e4f.PNG">

# CONFIGURE APACHE AS A LOAD BALANCER

## Configure Apache As A Load Balancer

1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:

<img width="728" alt="4" src="https://user-images.githubusercontent.com/29310552/161869830-883c5a8e-e1bf-4fc9-b5c2-09d3a31bc1ae.PNG">

2. Open a security group and enable the port 80 for TCP

3. Install Apache Load Balancer on the Load-Balancer server created and and configure it to point traffic coming to LB to both Web Servers.

Run the following command to configure the load balancer

```
#Install apache2
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev

#Enable following modules:
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic

#Restart apache2 service
sudo systemctl restart apache2

```

If trying to restart apache back and you encounter this error:

<img width="629" alt="error" src="https://user-images.githubusercontent.com/29310552/161876869-64fabcb4-9e6a-46d7-b035-165258521809.PNG">

<img width="704" alt="error1" src="https://user-images.githubusercontent.com/29310552/161876873-5ea188ad-06e5-4137-9a26-79d2d5c0b6dc.PNG">

Copy the index file into the `/var/www/html` and restart the apache server

<img width="502" alt="solution" src="https://user-images.githubusercontent.com/29310552/161876992-21d0c42c-30f8-4a21-ada2-82363a748caa.PNG">


Run the command below to check for the page

http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php
  
http://172.31.84.155/index.php
  
<img width="791" alt="10" src="https://user-images.githubusercontent.com/29310552/161877255-5e58dcb0-cb4f-4072-b634-a6e88ca77d5d.PNG">
  
To check the log in the terminal or ssh console

 
`$ sudo tail -f /var/log/httpd/access_log`
  
<img width="852" alt="11" src="https://user-images.githubusercontent.com/29310552/161877704-a0f09a8d-ead2-4179-bd30-59fbc5e0f6ac.PNG">
  
  
## Optional Step – Configure Local DNS Names Resolution
  
Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management.
What we can do, is to configure local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.
  
```
  
#Open this file on your LB server

sudo vi /etc/hosts

#Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers

<WebServer1-Private-IP-Address> Web1
<WebServer2-Private-IP-Address> Web2
  
```
  
<img width="464" alt="DNS" src="https://user-images.githubusercontent.com/29310552/161878733-69926fed-f952-4e5e-b738-8028b36648b8.PNG">
  
Re-run this command:

`$ sudo vi /etc/apache2/sites-available/000-default.conf`
  
Remove the private IP's within the file and replace with web1, web2 and web3 respectively
  
Run curl http://web1, curl http://web2 and curl http://web3
  
<img width="632" alt="last" src="https://user-images.githubusercontent.com/29310552/161879734-11f4b76e-c2ce-4526-9b41-6032fd38135e.PNG">

  
This project was done using the following materials from [Darey.io](https://www.darey.io/docs/32951-2/)


