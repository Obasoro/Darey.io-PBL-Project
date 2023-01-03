
## Developing MySQL in container

## Step 1: Pull MySQL Docker Image

Let us start assembling our application from the Database layer – we will use a pre-built MySQL database container, configure it, and make sure it is ready to receive requests from our PHP application

Make sure docker is installed on the system(locally or remote server)

`$ docker pull mysql/mysql-server:latest`

![image](https://user-images.githubusercontent.com/29310552/210363592-e58f6988-aee3-4697-9d57-477f3bc4bda0.png)

## Step 2: Deploy the MySQL Container to your Docker Engine

`$ docker run --name <container_name> -e MYSQL_ROOT_PASSWORD=<my-secret-pw> -d mysql/mysql-server:latest`

## CONNECTING TO THE MYSQL DOCKER CONTAINER

There are several approaches in connecting the MYSQL container.

- 1. Connecting directly to the container running the MySQL server

`$ docker exec -it mysql bash`

or

`$ docker exec -it mysql mysql -uroot -p`

Provide the root password when prompted. With that, you’ve connected the MySQL client to the server.

Finally, change the server root password to protect your database. Exit the the shell with exit command

Flags used

exec used to execute a command from bash itself
 -it makes the execution interactive and allocate a pseudo-TTY
bash this is a unix shell and its used as an entry-point to interact with our container
mysql The second mysql in the command "docker exec -it mysql mysql -uroot -p" serves as the entry point to interact with mysql container just like bash or sh
 -u mysql username
 -p mysql password

- 2. At this stage you are now able to create a docker container but we will need to add a network. So, stop and remove the previous mysql docker container.
The best practice is to store our password as enviroment variable and create our network environment.

First, create a network:

`$ docker network create --subnet=172.18.0.0/24 tooling_app_network`

![image](https://user-images.githubusercontent.com/29310552/210391701-8706cb1b-d2d6-4aff-843b-f9ae81075463.png)

You can check your network `docker network ls`

Creating a custom network is not necessary because even if we do not create a network, Docker will use the default network for all the containers you run. By default, the network we created above is of DRIVER Bridge. So, also, it is the default network

1. Let create a passowrd environment variable
`$ export MYSQL_PW=<password>`

`$ echo $MYSQL_PW`

2. `$ docker run --network tooling_app_network -h mysqlserverhost --name=mysql-server -e MYSQL_ROOT_PASSWORD=$MYSQL_PW  -d mysql/mysql-server:latest`

![image](https://user-images.githubusercontent.com/29310552/210393778-32597941-3aac-44fa-8273-a33a05dad80f.png)


Flags used

-d runs the container in detached mode
--network connects a container to a network
-h specifies a hostname
If the image is not found locally, it will be downloaded from the registry.

Verify the container is running:

![image](https://user-images.githubusercontent.com/29310552/210399390-c97d0209-96f7-4b4d-b5ae-29d885cbaf0b.png)

As you already know, it is best practice not to connect to the MySQL server remotely using the root user. Therefore, we will create an SQL script that will create a user we can use to connect remotely

Create a file and name it create_user.sql and add the below code in the file:

`$ CREATE USER ''@'%' IDENTIFIED BY ''; GRANT ALL PRIVILEGES ON * . * TO ''@'%';`

Run the script:
Ensure you are in the directory create_user.sql file is located or declare a path

`$ docker exec -i mysql-server mysql -uroot -p$MYSQL_PW < create_user.sql`

## Connecting to the MySQL server from a second container running the MySQL client utility

The good thing about this approach is that you do not have to install any client tool on your laptop, and you do not need to connect directly to the container running the MySQL server.

The good thing about this approach is that you do not have to install any client tool on your laptop, and you do not need to connect directly to the container running the MySQL server.

Run the MySQL Client Container:

`$ docker run --network tooling_app_network --name mysql-client -it --rm mysql mysql -h mysqlserverhost -u  -p`

Flags used:

--name gives the container a name
-it runs in interactive mode and Allocate a pseudo-TTY
--rm automatically removes the container when it exits
--network connects a container to a network
-h a MySQL flag specifying the MySQL server Container hostname
-u user created from the SQL script
admin username-for-user-created-from-the-SQL-script-create_user.sql
-p password specified for the user created from the SQL script


## Prepare database schema
Now you need to prepare a database schema so that the Tooling application can connect to it.

1. Clone the Tooling-app repository from
`$ $ git clone https://github.com/darey-devops/tooling.git`
2. On your terminal, export the location of the SQL file
`$ export tooling_db_schema=/tooling_db_schema.sql`



