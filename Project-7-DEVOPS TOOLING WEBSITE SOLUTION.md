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





















