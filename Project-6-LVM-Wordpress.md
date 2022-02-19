# Project 6
## Project Description
  - This project consist of creating two linux servers and implementing web solution [Wordpress](https://wordpress.com/). [WordPress](https://en.wikipedia.org/wiki/WordPress) is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

We would split the project workflow into this two parts to achieve this project
  - Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.
  - Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

As a DevOps Engineer we must have deep understanding of web solutions and how to troubleshoot them for any eventualities.

# Three Tier Architecture

Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

<strong>Three-tier Architecture</strong> is a client-server software architecture pattern that comprise of 3 separate layers.

  - <strong>Presentation Layer (PL)</strong>: This is the user interface such as the client server or browser on your laptop.
  - <strong>Business Layer (BL)</strong>: This is the backend program that implements business logic. Application or Webserver
  - <strong>Data Access or Management Layer (DAL)</strong>: This is the layer for computer data storage and data access

In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

For our three tier set-up:

- A Laptop or PC to serve as a client
- An EC2 Linux Server as a web server (This is where you will install WordPress)
- An EC2 Linux server as a database (DB) server

We would be using a <strong>Redhat</strong> Linux Distribution.

# Let Begin
