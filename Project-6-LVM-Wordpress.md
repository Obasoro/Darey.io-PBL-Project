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
























