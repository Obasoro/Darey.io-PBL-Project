# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

Load Balancing is used for and have configured an LB solution using Apache, but a DevOps engineer must be a versatile professional and know different alternative solutions for the same problem. That is why, in this project we will configure an Nginx Load Balancer solution.

There are different types of SSL/TLS certificates – you can learn more about them here. You can also watch a tutorial on how SSL works here or an additional resource here

In this project you will register your website with [LetsEnrcypt](https://letsencrypt.org/) Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt – [cetrbot](.

Task
This project consists of two parts:
 - Configure Nginx as a Load Balancer
 - Register a new domain name and configure secured connection using SSL/TLS certificates

![image](https://user-images.githubusercontent.com/29310552/163477077-5e19b097-2c20-44d8-b043-1df56fa16846.png)


# CONFIGURE NGINX AS A LOAD BALANCER

- Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is   used for secured HTTPS connections)
- 
- Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

- Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers


Run the following code below

`$ sudo apt update`
This code would install nginx server

`$ sudo apt install nginx`

Open the default nginx configuration file an paste the following file

`$ sudo vi /etc/nginx/nginx.conf`

```

#insert following configuration into http section

 upstream myproject {
    server Web1 weight=5;
    server Web2 weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
    location / {
      proxy_pass http://myproject;
    }
  }

#comment out this line
#       include /etc/nginx/sites-enabled/*;

```

Restart Nginx and make sure the service is up and running

`$ sudo systemctl restart nginx`

`$ sudo systemctl status nginx`

<img width="846" alt="2-nginx" src="https://user-images.githubusercontent.com/29310552/163490106-b7ed0c9f-8489-4d2d-95f9-29bdeb121bf2.PNG">

# REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

## STEPS
 - Register a new domain name with any registrar of your choice in any domain zone. Our domain was registered on [Whogohost](

<img width="557" alt="domain name" src="https://user-images.githubusercontent.com/29310552/163492627-0baf040f-21dd-4528-8bea-e9ab8c17dc55.PNG">

 - Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
 
 - Update A record in your registrar to point to Nginx LB using [Elastic IP](https://medium.com/progress-on-ios-development/connecting-an-ec2-instance-with-a-godaddy-domain-e74ff190c233) address or public IP as the case maybe
 
 <img width="610" alt="route53" src="https://user-images.githubusercontent.com/29310552/163492659-a275b48f-1b4d-4d43-af6f-d80997015994.PNG">

<img width="535" alt="nameserver" src="https://user-images.githubusercontent.com/29310552/163494617-1dfd083a-297c-4b13-88d4-e668efd90cda.PNG">

<img width="445" alt="namesserver1" src="https://user-images.githubusercontent.com/29310552/163494621-7ef8a350-ce39-4408-bf0c-7b25f16d8fed.PNG">

<img width="584" alt="route2" src="https://user-images.githubusercontent.com/29310552/163494623-1741754c-4c36-4465-9abd-7595f6bb473d.PNG">

<img width="815" alt="record1" src="https://user-images.githubusercontent.com/29310552/163495215-51870b98-cf79-4b0f-9740-698facca57f2.PNG">

<img width="820" alt="record2" src="https://user-images.githubusercontent.com/29310552/163495218-8984f123-20ee-4881-b083-1a7347a8dcb6.PNG">

- Configure Nginx to recognize your new domain name Update your nginx.conf with server_name www.<your-domain-name.com> instead of server_name (www.domain.com)

<img width="502" alt="server" src="https://user-images.githubusercontent.com/29310552/163496903-bd341cbc-7f8d-4951-9858-f2838a4dfd7f.PNG">

Run this command to confirm if nginx was successful installed

`$ sudo nginx -t`

<img width="472" alt="succ" src="https://user-images.githubusercontent.com/29310552/163509087-ead6760b-8ecf-4824-9cff-ea257f6c8aac.PNG">

- Configure Nginx to recognize your new domain name. [Nginx server name guide](http://nginx.org/en/docs/http/server_names.html)

<img width="782" alt="gmic" src="https://user-images.githubusercontent.com/29310552/163509654-96126f41-0a41-48ce-883f-4e9ed0dc9970.PNG">


- Install certbot and request for an SSL/TLS certificate. Make sure you have snapd 

`$ sudo apt install snapd`

<img width="537" alt="snapd" src="https://user-images.githubusercontent.com/29310552/163509635-44467a98-ecdd-4f75-9962-bb74979ebe88.PNG">

Install cerbort using the comman below:

`$ sudo snap install --classic certbot`

Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf 

<img width="634" alt="cert" src="https://user-images.githubusercontent.com/29310552/163510944-2ca240f9-0568-41d9-9c39-fa00ac1308b7.PNG">


With the command below we can check for certbot installation.

`$ sudo ln -s /snap/bin/certbot /usr/bin/certbot`

`$ sudo certbot --nginx`

We shall be able to access our website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string.


<img width="761" alt="gmic" src="https://user-images.githubusercontent.com/29310552/163511163-94b87ef0-fdf5-459d-afc3-87614a33fb35.PNG">

Set up periodical renewal of your SSL/TLS certificate
By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

You can test renewal command in dry-run mode

`sudo certbot renew --dry-run`

![image](https://user-images.githubusercontent.com/29310552/163511820-a4bf287f-2eed-49f5-8574-b54049946a5a.png)

The best practise is to schedule a the renewal date



`$ crontab -e`








