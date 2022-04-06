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


