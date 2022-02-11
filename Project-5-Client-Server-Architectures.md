<img width="774" alt="file-edit" src="https://user-images.githubusercontent.com/29310552/153532875-58ceaebc-fa31-4359-8669-763e4007153c.PNG">
# Understanding Client-Server Architecture

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another. It is also called of the <strong>“Client/Server Network” or “Network computing Model“</strong> because in this architecture all services and requests are spread over the network. Its functionality like as distributed computing system because in which all components are performing their tasks independently from each other.
There are various layers of architecture. For further reading check [Client-Server](https://digitalthinkerhelp.com/what-is-client-server-architecture-diagram-types-examples-components/)

# Step 1

A <strong>Client</strong> and <strong>Server</strong> E2 instances were created.

<img width="698" alt="2nd-" src="https://user-images.githubusercontent.com/29310552/153525826-ffe98068-5b66-4469-94cc-f03175d02f4a.PNG">

We can use `curl` to access the server from the client. We are using www.propitixhomes.com as our server and any of newly created instance as Client.

<img width="860" alt="Ist" src="https://user-images.githubusercontent.com/29310552/153526015-d8308203-4300-4665-be0c-672e8846a935.PNG">

On the <strong>server</strong>, run the following command to update

`$ sudo apt-get update`

`$ sudi apt install mysql-server -y

<img width="649" alt="3rd" src="https://user-images.githubusercontent.com/29310552/153527157-fd752dd2-f175-4ff2-a50a-051ded7c581c.PNG">


<img width="512" alt="4th" src="https://user-images.githubusercontent.com/29310552/153527161-1fbbafeb-fc32-4aa4-9ea0-7ca479a8114c.PNG">

Run this command to enable mysql-server

`sudo systemctl enable mysql-server`

<img width="729" alt="5" src="https://user-images.githubusercontent.com/29310552/153528329-33743d3c-3251-4159-8048-613f7ab24932.PNG">

Get the IP address of the remote server

`$ ip addr show`

<img width="713" alt="ip -addre" src="https://user-images.githubusercontent.com/29310552/153534910-d82785ce-fb5b-4061-a578-ca82f24634c7.PNG">


SSH into the Client Server

<img width="850" alt="clinet2" src="https://user-images.githubusercontent.com/29310552/153528509-19177c13-2281-4a9e-8cfc-aabe43198eb5.PNG">


<img width="701" alt="client" src="https://user-images.githubusercontent.com/29310552/153528313-70a56541-b6aa-44db-b3c9-5bfff02b6409.PNG">

By default, both of our EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

<img width="825" alt="client-ip" src="https://user-images.githubusercontent.com/29310552/153529672-1ba0fc9e-ab21-47e1-87e1-3959d039e4d4.PNG">

ou might need to configure MySQL server to allow connections from remote hosts.

On the mysql -DB server run this command

`$ sudo mysql_secure_installation`

When prompted with password option: Type any key for No.

Type yes for other option or as it best fit your need

<img width="937" alt="mysql instal" src="https://user-images.githubusercontent.com/29310552/153530869-0b00c220-0036-46fd-9ed1-f1b12cbe1c16.PNG">

`$ sudo mysql`

Create a database, grant privileges abd flush othe privileges

<img width="952" alt="mysql instali" src="https://user-images.githubusercontent.com/29310552/153531979-a3f7623e-55f6-4c1c-ad9e-62d34c8d0495.PNG">

Run this command

`$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

Go to the binder and change the IP nunber given there to 0.0.0.0, to allow access from divers IP address

Restart mysql

`$ sudo systemctl restart mysql`

We shall connect from the Client server into the DB by remotely connectin through the IP address

`<img width="637" alt="enter-mysql" src="https://user-images.githubusercontent.com/29310552/153534681-1fc7042c-2be1-47c7-8d83-fce92f95d046.PNG">

sudo mysql -u remote_user -h 172.31.95.51 -p`

Test whether our schema was created

<img width="620" alt="schema" src="https://user-images.githubusercontent.com/29310552/153534736-552ac0b9-7f59-4089-845a-a4b3827e9ac6.PNG">








