# DEVOPS TOOLING WEBSITE SOLUTION

## STEP 1 – PREPARE NFS SERVER

 In this project we implemented a solution that consists of following components:

 - Infrastructure: AWS
 - Webserver Linux: Red Hat Enterprise Linux 8
 - Database Server: Ubuntu 20.04 + MySQL
 - Storage Server: Red Hat Enterprise Linux 8 + NFS Server
 - Programming Language: PHP
 - Code Repository: [GitHub](https://github.com/darey-io/tooling.git)


# Preamble

We would be using the architetcure below to design 

![image](https://user-images.githubusercontent.com/29310552/157105119-45746cb0-7eb5-45c3-ac04-b92d98f28a49.png)

We created four webservers abd one database server base on the architecture above

<img width="762" alt="1" src="https://user-images.githubusercontent.com/29310552/157112284-23b78f5d-10b1-4d08-842a-c3123b725f3d.PNG">

Volume were cretaed base on [Project 6](https://dareyio-pbl-progressive.readthedocs-hosted.com/en/latest/project6.html)

<img width="756" alt="2" src="https://user-images.githubusercontent.com/29310552/157112439-9755edc7-6eda-4be2-bbac-e1c2ba9248fa.PNG">

Connect to your NFS-server using ssh

<img width="856" alt="3" src="https://user-images.githubusercontent.com/29310552/157112567-44d76ae0-14e3-4832-ba1d-2d072ae57e9b.PNG">

Use the `lsblk` command to inspect the block devices attached to server

<img width="485" alt="lbk" src="https://user-images.githubusercontent.com/29310552/157117600-4157695a-d815-490f-8221-fc2734e69c96.PNG">

Let us create the Logical Volumes (LVM) on the attached volumes on the web-server

`$ sudo gdisk /dev/xvdf`

<img width="371" alt="5" src="https://user-images.githubusercontent.com/29310552/157114710-748d2b09-ffc0-4b8a-90ac-ea74288a2929.PNG">

<img width="577" alt="vxdf-6" src="https://user-images.githubusercontent.com/29310552/157117960-832e069b-098c-49f3-bbd2-af870e436832.PNG">

$ sudo gdisk /dev/xvdh

<img width="596" alt="xvdh-7" src="https://user-images.githubusercontent.com/29310552/157118276-924f27e1-29fa-4837-bbb4-0478868af514.PNG">

Install LVM on the Redhat distro

`$ sudo yum install lvm2`

<img width="855" alt="8-" src="https://user-images.githubusercontent.com/29310552/157119116-23a64800-9e50-4ecd-bbbe-5f3e7377a620.PNG">

`$ sudo lvmdiskcan`

<img width="520" alt="9" src="https://user-images.githubusercontent.com/29310552/157119362-fbfbde06-89b6-402d-8fbb-2d927a036c2d.PNG">

To confirm the the partitionhas been created:

`$ sudo lsblk`

<img width="386" alt="10" src="https://user-images.githubusercontent.com/29310552/157119696-bc925c1e-cca8-40e6-898b-1797eeeeb65d.PNG">

Run the following commands to create physical volumes:

`$ sudo pvcreate /dev/xvdf1`

`$ sudo pvcreate /dev/xvdg1`

`$ sudo pvcreate /dev/xvdh1`

<img width="465" alt="11" src="https://user-images.githubusercontent.com/29310552/157120179-484ff4a8-9437-4ecc-9b96-b13890fe92a5.PNG">

To confirm that our physical volumes have been successful we run

`$ sudo pvs`

<img width="387" alt="12" src="https://user-images.githubusercontent.com/29310552/157120383-8394ebed-58f7-4fbf-a10a-c39b930525db.PNG">

`$ sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`

<img width="649" alt="13" src="https://user-images.githubusercontent.com/29310552/157121180-8c08232d-65d7-4514-99cf-ba05d75dcbf6.PNG">

We would use lvcreate utility to create 3 logical volumes. lv-apps, lv-logs and lv-opt. NOTE: The lv-opt will be used in project 8.

`$ sudo lvcreate -n lv-apps -L 9G webdata-vg`

`$ sudo lvcreate -n lv-logs -L 9G webdata-vg`

`$ sudo lvcreate -n lv-opt -L 9G webdata-vg`

<img width="639" alt="14" src="https://user-images.githubusercontent.com/29310552/157124817-a6826858-37a2-4ff3-87de-5fa6f2530282.PNG">

`$ sudo vgdisplay -v #view complete setup - VG, PV, and LV`

<img width="751" alt="15" src="https://user-images.githubusercontent.com/29310552/157124895-d67d668e-4558-414b-b813-2c30380faa9c.PNG">

Formatting the physical volumes using xfs

`$ sudo mkfs -t xfs /dev/webdata-vg/lv-logs`

`$ sudo mkfs -t xfs /dev/webdata-vg/lv-apps`

<img width="579" alt="16" src="https://user-images.githubusercontent.com/29310552/157126052-84986572-eb60-4bf2-a2a5-0d0eb58487cf.PNG">

Create a directory apps, logs, and opt and mount the lv-logs, lv-apps and lv-opt

`$ sudo mkdir /mnt/apps`

`$ sudo mkdir /mnt/logs`

`$ sudo mkdir/mnt/opt`

Now mount the lv-apps, lv-logs and lv-opt

<img width="607" alt="17" src="https://user-images.githubusercontent.com/29310552/157127719-78faf920-2a53-4e79-a918-e7557fcf8996.PNG">

On the NFT-Server, run the following commands:

`$ sudo yum -y update`

`$ sudo yum install nfs-utils -y`

`$ sudo systemctl start nfs-server.service`

`$ sudo systemctl enable nfs-server.service`

`$ sudo systemctl status nfs-server.service`

<img width="733" alt="18" src="https://user-images.githubusercontent.com/29310552/157131123-989b715a-486f-4429-89a3-1f8cd26cfba9.PNG">

Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, you would install all three Web Servers inside the same subnet, but in production set up we would probably want to separate each tier inside its our subnet for higher level of security.
To check our subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:

<img width="757" alt="web-1" src="https://user-images.githubusercontent.com/29310552/157135471-19da3116-13ef-438a-9364-e8cbbdad810e.PNG">

we now set up permission that will allow our Web servers to read, write and execute files on NFS:

`$ sudo chown -R nobody: /mnt/apps`
`$ sudo chown -R nobody: /mnt/logs`
`$ sudo chown -R nobody: /mnt/opt`

`$ sudo chmod -R 777 /mnt/apps`
`$ sudo chmod -R 777 /mnt/logs`
`$ sudo chmod -R 777 /mnt/opt`

`$ sudo systemctl restart nfs-server.service`

<img width="613" alt="19" src="https://user-images.githubusercontent.com/29310552/157137429-b0348fbd-015a-4212-a255-1c889e6ac6c6.PNG">

Run this command below

`$ sudo vi /etc/exports`

Paste this inside the text editor and save

```
/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

```
<img width="439" alt="20" src="https://user-images.githubusercontent.com/29310552/157137675-b8f7082b-ac08-4a9e-8eef-69f416a2e25d.PNG">

`$ sudo expoertfs -arv`

`$rpcinfo -p | grep nfs

<img width="436" alt="21" src="https://user-images.githubusercontent.com/29310552/157138376-e728fff9-012d-4436-a459-699c5b8974ee.PNG">


Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

<img width="694" alt="security-group" src="https://user-images.githubusercontent.com/29310552/157138507-897abb3c-919e-4cd9-b966-20747794ab4d.PNG">

## Configure Databases Server

By now you should know how to install and configure a MySQL DBMS to work with remote Web Server

 - Install MySQL server
 - Create a database and name it tooling
 - Create a database user and name it webaccess
 - Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

<img width="734" alt="ubuntu-db" src="https://user-images.githubusercontent.com/29310552/157138848-9eb907fa-8fdb-4822-ac61-7b12a5eb81ec.PNG">

<img width="569" alt="ubuntu-db1" src="https://user-images.githubusercontent.com/29310552/157139072-27195207-4a86-43dd-83f2-50c5dc4195ba.PNG">

<img width="233" alt="ubuntu-db2" src="https://user-images.githubusercontent.com/29310552/157139073-486dfbc3-4cc2-4aae-95f7-bda1a31713d3.PNG">

## Step 3 — Prepare the Web Servers

During the next steps we will do following:

- Configure NFS client (this step must be done on all three servers)
- Deploy a Tooling application to our Web Servers into a shared NFS folder
- Configure the Web Servers to work with a single MySQL database

`$ sudo yum install nfs-utils nfs4-acl-tools -y`

4. Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:

`$ df -h

<img width="548" alt="webserver1" src="https://user-images.githubusercontent.com/29310552/157140779-eeac05ad-9bd1-4b47-b9f6-5b02f1a092c3.PNG">

`$ sudo vi /etc/fstab`

Add this file below:

```
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0

```

`$ sudo vi /etc/fstab`

<img width="663" alt="webserver2" src="https://user-images.githubusercontent.com/29310552/157141687-e47cdd2a-4c04-4b75-b9c5-b604e1e385f9.PNG">

5. Install Remi’s repository, Apache and PHP
```
`$ sudo yum install httpd -y`

`$ sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`

`$ sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`

`$ sudo dnf module reset php`

`$ sudo dnf module enable php:remi-7.4`

`$ sudo dnf install php php-opcache php-gd php-curl php-mysqlnd`

`$ sudo systemctl start php-fpm`

`$ sudo systemctl enable php-fpm`

`$ setsebool -P httpd_execmem 1`

```



























