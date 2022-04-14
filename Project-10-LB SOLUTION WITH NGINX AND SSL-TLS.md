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


