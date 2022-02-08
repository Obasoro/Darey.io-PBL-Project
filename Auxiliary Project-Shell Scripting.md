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

Running the code below we can access the developer directory from the root user

`# ls -l`
The code would look like this:<img width="304" alt="cat" src="https://user-images.githubusercontent.com/29310552/153015119-57b79f80-a2ab-48a7-b044-743a8b50a87a.PNG">

`/home/ubuntu/Shell# ls -l /home/`

<img width="682" alt="dr-deve" src="https://user-images.githubusercontent.com/29310552/153012767-783df646-dec6-4604-baf8-94a5b49c23f3.PNG">

To check for developers group

`$ getent group developers`

<img width="470" alt="listof develpe" src="https://user-images.githubusercontent.com/29310552/153050297-2b2b7d70-fe2a-4028-b74b-def12595b084.PNG">


` cat /etc/passwd`

<img width="472" alt="passwd" src="https://user-images.githubusercontent.com/29310552/153050528-1e4d9e81-e689-44e8-8290-3377f64de2a4.PNG">



Run the following code

ASWK is used for filtering and test

`cat /etc/passwd | awk -F':' '{ print $1}' | xargs -n1 groups`
<img width="464" alt="cat" src="https://user-images.githubusercontent.com/29310552/153051039-8c7ed32e-d528-4c17-8c8f-bd09ab6fb59a.PNG">



Created a new file called Auxillairy-project.pem on windows desktop
```
-----BEGIN OPENSSH PRIVATE KEY-----
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
kmaSGj362OnCCNAAAACWRhcmVARGFyZQE=
-----END OPENSSH PRIVATE KEY-----
```











