
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


