# Auxillairy Project - Shell Scripting

In this project is to create a CSV file, which contains 20 Developers who are to be onboarded in a firm.

Create the instance on EC2

<img width="754" alt="instance created" src="https://user-images.githubusercontent.com/29310552/153002265-daccfc9c-82f7-4b35-8531-d2dfd91e0734.PNG">

Connect our EC2

<img width="586" alt="Capture-1" src="https://user-images.githubusercontent.com/29310552/153003221-37a57be1-6f49-457c-8c86-54af5d1a4315.PNG">

<img width="637" alt="Capture2" src="https://user-images.githubusercontent.com/29310552/153003228-eac984ab-4d98-4ea0-b3b6-0604ecd584d3.PNG">

<img width="834" alt="Capture3" src="https://user-images.githubusercontent.com/29310552/153003233-09d3f02d-cc33-486e-b466-4b032b2c5b57.PNG">



I copied a Shell Script from [Youtube](https://www.youtube.com/watch?v=NysMJ9rhVIU)


<img width="455" alt="script" src="https://user-images.githubusercontent.com/29310552/153000888-f7b1d51a-d297-45af-94e4-10d5f93e2082.PNG">

To begin, make a directory

`$ mkdir Shell`

Cd in to the directory

`$ cd Shell`

Create the follwing within the Shell folder

`$ touch id_rsa id_rsa.pub names.csv
Public Key
`$ vi id_rsa
<img width="682" alt="rsapub" src="https://user-images.githubusercontent.com/29310552/153006426-17b9c6d9-f4b0-4e8b-ab21-0275e80b1a88.PNG">
Private Key
`$ vi id_rsa.pub`
<img width="680" alt="pr key" src="https://user-images.githubusercontent.com/29310552/153006628-abe57ae5-d8fa-43c0-b50f-f718270572be.PNG">
Nmaes
`$ vi names.csv`
<img width="683" alt="names" src="https://user-images.githubusercontent.com/29310552/153006744-a236589a-7221-49e4-ad1a-83c9884b3c1a.PNG">

Copy the working directory of the Shell folder and paste within the onboard.sh file

<img width="631" alt="update" src="https://user-images.githubusercontent.com/29310552/153007759-47b09bd5-d87a-48ba-a6ca-4f888fe31b40.PNG">

Create a group within the

`$ sudo groupadd developers`

To allow for exectable and accessible

`$ sudo chmod +x onboard.sh`

<img width="681" alt="test" src="https://user-images.githubusercontent.com/29310552/153009236-415e43a1-d88e-4c2b-a243-eddfba8205e6.PNG">

To check access if you have access as root user

`$ sudo su -`
<img width="677" alt="su-su" src="https://user-images.githubusercontent.com/29310552/153010928-bea33508-d7f8-4849-9831-08e774b13b6f.PNG">

`$ sudo su`

<img width="675" alt="susu" src="https://user-images.githubusercontent.com/29310552/153011189-2e4d5e95-6d7c-4681-bbb3-46d8f42adc8a.PNG">

Running the code below we can access the developer directory

`# ls -l`
The code would look like this:
`/home/ubuntu/Shell# ls -l /home/`







