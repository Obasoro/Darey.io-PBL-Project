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

# Create two EC2 Server and name them accordingly

<img width="773" alt="EC2 creted" src="https://user-images.githubusercontent.com/29310552/154798552-838f7491-0941-479e-900d-52db94aa020c.PNG">

Create volumes to be attached

<img width="763" alt="volume-created-2" src="https://user-images.githubusercontent.com/29310552/154798589-de335158-84fd-4d6c-8ba5-55c270cc84d2.PNG">

Attach the created volumes to EC2 web-server

<img width="681" alt="attache 3" src="https://user-images.githubusercontent.com/29310552/154798600-ec21a060-37a2-41cd-817f-37c9670477d2.PNG">

SSH into the server

<img width="616" alt="ssh4" src="https://user-images.githubusercontent.com/29310552/154798634-93395077-1784-4254-a48d-fef1db9a3e25.PNG">

Fully connected to the Instance from your terminal

<img width="863" alt="5" src="https://user-images.githubusercontent.com/29310552/154798674-abdb3383-1e4c-4ab2-855a-a40d93f6cbeb.PNG">

<img width="859" alt="6" src="https://user-images.githubusercontent.com/29310552/154798727-7cb851cd-abd5-4973-b72a-aeb4e926bfe6.PNG">

Use the `lsblk` command to inspect the block devices attached to server

<img width="433" alt="7" src="https://user-images.githubusercontent.com/29310552/154798797-85bc362c-0052-448a-9bff-3fffaa1b6969.PNG">

`df -h` command to view the free space within server disk space.

<img width="547" alt="8" src="https://user-images.githubusercontent.com/29310552/154798804-dd97e408-1a77-4308-b26a-3a8bff1b5d21.PNG">

Let us create the Logical Volumes (LVM) on the attached volumes on the web-server

`$ sudo gdisk /dev/xvdf`

<img width="614" alt="10" src="https://user-images.githubusercontent.com/29310552/154800300-e10cda2a-a3bc-4c54-9449-f5df49084982.PNG">

Running these various command `n` for number of partition

You can press enter/return to choose the whole space ot specify using `+(number of space)`

You can check the Logical volume state press l to see the LVM code which is 8e00

Press `p` to confirm partition and w to write.

Type `yes` to check if the partition is successful

<img width="944" alt="11" src="https://user-images.githubusercontent.com/29310552/154800431-b1998d85-b402-4727-a947-4c87aaa5e2d8.PNG">

You can repeat the same process for all created volume

<img width="818" alt="12" src="https://user-images.githubusercontent.com/29310552/154800449-8ad7e258-29e5-43e2-a411-9abc4c08b362.PNG">

Install LVM on the Redhat distro

`$ sudo yum install lvm2`

<img width="844" alt="13-install lvm" src="https://user-images.githubusercontent.com/29310552/154800984-7182ca13-b46d-4909-b93f-f1e1e39e4855.PNG">

To check if the `LVM` is installed

`$ sudo lvmdiskscan`

<img width="559" alt="14-partion" src="https://user-images.githubusercontent.com/29310552/154801035-b60be606-9a8a-4c04-8f96-1604f011f46e.PNG">

We are going to create the physical volume on the LVM. Run the following commands

`$ sudo pvcreate /dev/xvdf1`

`$ sudo pvcreate /dev/xvdh1`

`$ sudo pvcreate /dev/xvdg1`

<img width="607" alt="15-phys-vol" src="https://user-images.githubusercontent.com/29310552/154801281-2e7c1f86-d7e5-4ee3-94d1-be3413cb109b.PNG">

To confirm that our physical volumes have been successful we run

`$ sudo pvs`

<img width="421" alt="16-confirm" src="https://user-images.githubusercontent.com/29310552/154801309-93190e75-c498-490b-b519-57a10fc9238b.PNG">


We use vgcreate to add the lvm into a volume group and name the volume group as webdata-vg

`$ sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`

<img width="661" alt="17-vg" src="https://user-images.githubusercontent.com/29310552/154801519-772f78ea-d82e-447e-bc13-342a5254b624.PNG">

To confirm if the volume group was successful, run this command

`$ sudo vgs`

<img width="548" alt="18-success" src="https://user-images.githubusercontent.com/29310552/154801540-8e06acd9-161d-429b-b3c4-781a576342ba.PNG">

W would use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

`$ sudo lvcreate -n apps-lv -L 14G webdata-vg`

`$ sudo lvcreate -n logs-lv -L 14G webdata-vg`

<img width="577" alt="19-volumes created" src="https://user-images.githubusercontent.com/29310552/154801682-2e51485c-ef1c-41c0-bf53-03fe8b0c7006.PNG">

Verify if logical volume are now created

<img width="657" alt="20-lvs" src="https://user-images.githubusercontent.com/29310552/154801747-6b5653dc-106c-4077-89b8-904ba1821f8a.PNG">

Verify the whole set-up

`$ sudo vgdisplay -v #view complete setup - VG, PV, and LV`

<img width="642" alt="21-" src="https://user-images.githubusercontent.com/29310552/154801863-042ef4be-67a5-4a47-b21e-a508d62a0374.PNG">

For full display of partition

`$ sudo lsblk`

<img width="476" alt="22-" src="https://user-images.githubusercontent.com/29310552/154801895-f4d80f9e-3177-4c58-8a0a-9318d4bef29f.PNG">

We would use the mkfs.ext4 to format the logical volumes with ext4 filesystem

`$ sudo mkfs -t ext4 /dev/webdata-vg/apps-lv`

`$ sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`

<img width="569" alt="23-format" src="https://user-images.githubusercontent.com/29310552/154802069-6cc5491e-1ef3-4826-8021-9269d4757d17.PNG">

Create /var/www/html directory to store website files

Create /home/recovery/logs to store backup of log data

Mount /var/www/html on apps-lv logical volume

Use [rsync](https://linux.die.net/man/1/rsync) utility to backup all the files in the log directory /var/log into /home/recovery/logs

<img width="728" alt="24-" src="https://user-images.githubusercontent.com/29310552/154802337-134af727-b8ff-49a8-8c5f-125fdf13275e.PNG">

Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very
important)

Restore log files back into /var/log directory

Update /etc/fstab file so that the mount configuration will persist after restart of the server.

`$ sudo blkid`

<img width="865" alt="25-" src="https://user-images.githubusercontent.com/29310552/154819500-c289f26a-f9fe-41f5-8ca6-d7c04bab59e8.PNG">

UPDATE THE `/ETC/FSTAB` FILE

The UUID of the device will be used to update the /etc/fstab file

`$ sudo vi /etc/fstab`

<img width="657" alt="26" src="https://user-images.githubusercontent.com/29310552/154820089-e44cbae9-c673-47ed-acf4-980c657f8610.PNG">

Run the following command to Test the configuration and reload the daemon

`$ sudo mount -a`
`$ sudo systemctl daemon-reload`

<img width="688" alt="27-" src="https://user-images.githubusercontent.com/29310552/154820171-f10e9cd6-4886-4414-8b15-cab9e0aa811f.PNG">

# Step 2 — Prepare the Database Server
In the first stage we prepared the webserver, now we would need to prepare the database server

Create volumes and and attached the volume to database server

<img width="760" alt="1" src="https://user-images.githubusercontent.com/29310552/154821641-070c6310-bb2a-4190-8936-d10ef3bbde06.PNG">

Access the instance through SSH

<img width="860" alt="2" src="https://user-images.githubusercontent.com/29310552/154822032-ab711797-7e9b-4255-a32d-36c63e430263.PNG">

Check the added volumes in the instance

<img width="350" alt="3" src="https://user-images.githubusercontent.com/29310552/154822045-18cd3d26-520d-47da-a137-2c65aef0dcfe.PNG">

Create a Logical volume partitions within the disk using the gdisk

`$ sudo gdisk /dev/xvdf`

<img width="646" alt="4-" src="https://user-images.githubusercontent.com/29310552/154822077-085aaf23-d7d7-4bd8-8112-b7043259cbfe.PNG">

`$ sudo gdisk /dev/xvdg`

<img width="709" alt="5-xvdg" src="https://user-images.githubusercontent.com/29310552/154822086-a58310a4-3ab6-4375-b874-5d53a9434500.PNG">

`$ sudo gdisk /dev/xvdh`

<img width="725" alt="6-h" src="https://user-images.githubusercontent.com/29310552/154822089-2037c27c-ea58-4598-bf72-465679265cfa.PNG">

To confirm logical volume, run this command
`$ lsblk`

<img width="401" alt="7-" src="https://user-images.githubusercontent.com/29310552/154822251-56af830c-66c7-4f3c-bfbc-be0d07e7af58.PNG">

We now install logical volume using this command on Redhat distro

`$ sudo yum install lvm2`

<img width="858" alt="8-install lvm" src="https://user-images.githubusercontent.com/29310552/154822356-5205c2f0-8b7e-44b0-bd3f-217b73bc0640.PNG">


To check the available partitions

`$ sudo lvmdiskscan`

<img width="421" alt="9-lvmdisk" src="https://user-images.githubusercontent.com/29310552/154822352-85dab90e-77ec-4078-8b87-1c5f6e93def6.PNG">

Use [pvcreate](https://linux.die.net/man/8/pvcreate) utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

`$ sudo pvcreate /dev/xvdf1`

`$ sudo pvcreate /dev/xvdg1`

`$ sudo pvcreate /dev/xvdh1`

<img width="472" alt="10-physcal" src="https://user-images.githubusercontent.com/29310552/154822413-d4c7a879-0120-420b-9300-01578d6260ff.PNG">

Verify that your Physical volume has been created successfully by running

<img width="351" alt="11-pvs" src="https://user-images.githubusercontent.com/29310552/154822472-2287a32e-28d2-4ff3-988c-0ad10587daad.PNG">

Use [vgcreate](https://linux.die.net/man/8/vgcreate) utility to add all 3 PVs to a volume group (VG). Name the VG vg-database

`$ sudo vgcreate vg-database /dev/xvdf1 /dev/xvdg1 /dev/xvdh1`

`$ sudo vgs`

<img width="702" alt="12-" src="https://user-images.githubusercontent.com/29310552/154822680-861eb8f3-bbb7-4701-b56b-4305ef10c1b6.PNG">

To create

`$ sudo lvcreate -n db-lv -L 25G vg-database`

<img width="571" alt="13-vgs" src="https://user-images.githubusercontent.com/29310552/154823021-2cad9e81-d625-4e0f-8d34-c1e568cada08.PNG">

Verify the entire setup

`$ sudo vgdisplay -v #view complete setup - VG, PV, and LV`

<img width="803" alt="14-dis" src="https://user-images.githubusercontent.com/29310552/154823096-fb124b7f-9865-41e0-839c-2a032138027e.PNG">


`$ sudo lsblk`

<img width="475" alt="15-lsb" src="https://user-images.githubusercontent.com/29310552/154823104-d6599b44-3ea2-4611-8ca0-8ee724c063e0.PNG">

Use mkfs.ext4 to format the logical volumes with ext4 filesystem

First create a directory for database

`$ sudo mkdir /db

`$ sudo mkfs.ext4 /dev/vg-database/dv-lv`

<img width="639" alt="16-db" src="https://user-images.githubusercontent.com/29310552/154823360-f27d8e31-2efe-47f8-8c38-6db1020e96d8.PNG">

`sudo mount /dev/vg-database/db-lv /db`

<img width="928" alt="17-" src="https://user-images.githubusercontent.com/29310552/154823747-e02f2adc-14c0-43b6-8509-c3d5743626e7.PNG">

Insert the UUID number and reload the system

`$ sudo vi /etc/fstab

<img width="681" alt="18-" src="https://user-images.githubusercontent.com/29310552/154823909-25d4e187-29c1-41c9-8e18-3e2927f8671c.PNG">

`$ sudo mount -a

`$ sudo systemctl reload`

`$ df -h`

<img width="489" alt="19" src="https://user-images.githubusercontent.com/29310552/154824001-2de874f1-27ab-46e3-aa7e-c86dc9aa9ba7.PNG">

# Step 3 — Install WordPress on your Web Server EC2

-Update the repository

`$ sudo yum -y update`

-Install wget, Apache and it’s dependencies
[tecmint](https://www.tecmint.com/install-php-8-on-centos/) on how to install php on redhat

`$ sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

-#To install PHP and it’s depemdencies
`$ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`
`$ sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`
`$ sudo yum module list php`
`$ sudo yum module reset php`
`$ sudo yum module enable php:remi-7.4`
`$ sudo yum install php php-opcache php-gd php-curl php-mysqlnd`
`$ sudo systemctl start php-fpm`
`$ sudo systemctl enable php-fpm`
`$ setsebool -P httpd_execmem 1`

<img width="831" alt="21-modul" src="https://user-images.githubusercontent.com/29310552/154824885-adaa0655-0ce8-49e7-9fcf-10c4975205d5.PNG">

<img width="955" alt="22" src="https://user-images.githubusercontent.com/29310552/154824887-2d5d01eb-69da-424d-9936-2e0b50107498.PNG">


-Start Apache
`$ sudo systemctl enable httpd`
`$ sudo systemctl start httpd`

<img width="871" alt="23" src="https://user-images.githubusercontent.com/29310552/154824881-02ce7564-acfa-4ed6-8718-bfd6aca10177.PNG">


`$ sudo systemctl restart httpd`

To test if the apache is running

<img width="848" alt="24-" src="https://user-images.githubusercontent.com/29310552/154825171-f7869eca-3775-409d-96cb-a9e096f4b056.PNG">

- Download wordpress and copy wordpress to var/www/html

`$ mkdir wordpress`
`$ cd   wordpress`
`$ sudo wget http://wordpress.org/latest.tar.gz`
`$ sudo tar xzvf latest.tar.gz`
`$ sudo rm -rf latest.tar.gz`
`$ cp wordpress/wp-config-sample.php wordpress/wp-config.php`
~$ cp -R wordpress /var/www/html/`

<img width="902" alt="26-" src="https://user-images.githubusercontent.com/29310552/154825677-31b0aca2-716f-4bc0-b865-a709ac2e3b89.PNG">

<img width="821" alt="27-" src="https://user-images.githubusercontent.com/29310552/154825678-e4d2fba7-a6c4-48f4-99b1-7d59257d20cf.PNG">

<img width="735" alt="28-" src="https://user-images.githubusercontent.com/29310552/154825679-ab167a2b-b449-481a-9a9a-1e3b33eb7225.PNG">

Also install `mysql` within the webserver. This would enble the wordpress store the data

`$ sudo yum install mysql-server -y

`$ sudo systemctl start mysqld`

`$ sudo systemctl status mysqld`

<img width="829" alt="29" src="https://user-images.githubusercontent.com/29310552/154825921-e9c090bd-6bb4-476e-80c3-ef8aa6baa5b4.PNG">

Repeat the same process for the database server

`$ sudo yum install mysql-server -y

`$ sudo systemctl start mysqld`

`$ sudo systemctl status mysqld`

`$ sudo mysql_secure_installation`

`$ sudo mysql -u root -p`

<img width="692" alt="30-" src="https://user-images.githubusercontent.com/29310552/154826107-4cfcb121-f433-4a4c-90cd-d682569fb51f.PNG">


<img width="734" alt="31" src="https://user-images.githubusercontent.com/29310552/154826110-8e2b831a-c57a-4eb0-9923-ef9d35f04129.PNG">


Run this command and change the name of database to one you used, private IP address from EC2 and also password.

`$ sudo vi wp-config-sample.php`

This command is to disable the apache page

`$ sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup`


`$ sudo mysql -h 172.31.27.234 -u wordpress -p`

<img width="959" alt="32" src="https://user-images.githubusercontent.com/29310552/154827196-fb93c9e7-11a7-48d5-bbc7-34c9bcced0c2.PNG">


Configure SELinux Policies

`$  sudo chown -R apache:apache /var/www/html/`

<img width="740" alt="33-root-apache" src="https://user-images.githubusercontent.com/29310552/154827285-107261cd-8623-4cda-a9c6-9a69b4320f7d.PNG">

`$ sudo chcon -t httpd_sys_rw_content_t /var/www/html/ -R`
  
`$ sudo setsebool -P httpd_can_network_connect=1`

`$ sudo setsebool -P httpd_can_network_connect_db 1`

Reload the webserver Instance

<img width="909" alt="34" src="https://user-images.githubusercontent.com/29310552/154827711-f4a008b6-117d-4220-abea-57cd8df899fd.PNG">

<img width="931" alt="35" src="https://user-images.githubusercontent.com/29310552/154827731-1be39a9a-a33e-4f43-9e2f-b478ea3ef939.PNG">

  


























































