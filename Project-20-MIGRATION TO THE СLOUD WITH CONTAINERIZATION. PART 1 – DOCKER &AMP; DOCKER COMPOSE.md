
## Developing MySQL in container

## Step 1: Pull MySQL Docker Image

Let us start assembling our application from the Database layer – we will use a pre-built MySQL database container, configure it, and make sure it is ready to receive requests from our PHP application

Make sure docker is installed on the system(locally or remote server)

`$ docker pull mysql/mysql-server:latest`

![image](https://user-images.githubusercontent.com/29310552/210363592-e58f6988-aee3-4697-9d57-477f3bc4bda0.png)

## Step 2: Deploy the MySQL Container to your Docker Engine

`$ docker run --name <container_name> -e MYSQL_ROOT_PASSWORD=<my-secret-pw> -d mysql/mysql-server:latest`



