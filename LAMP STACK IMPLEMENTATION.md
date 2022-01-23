# PROJECT 1 - LAMP STACK IMPLEMENTATION

## Steps involved in implementation
---------------------------------------

### Step 1: Installing apache and updating the firewall
__________________________________________________________

update a list of packages in package manager

`$ sudo apt update`


![Apache1](https://user-images.githubusercontent.com/29310552/150621426-6461bd4a-80f9-441b-b5d3-46c60310814d.JPG)

run apache2 package installation

`$ sudo apt install apache2`

![Apach2](https://user-images.githubusercontent.com/29310552/150621532-925c2323-8209-465a-81d7-144686769317.JPG)

run the following command 

`$ sudo systemctl status apache2`
![Apache3runn](https://user-images.githubusercontent.com/29310552/150621618-c4bf58f6-1044-4f78-a7c4-34329ed7f9f9.JPG)

On the AWS console the imbound rules was adjusted to *Anywhere* to receive traffic
![Imbound Rules](https://user-images.githubusercontent.com/29310552/150621738-75ddceba-9d5e-48c4-b70c-ed282d2c0489.JPG)

#To run the apache2 locally use the command below:

`$ curl http://localhost:80`

or

`$ curl http://127.0.0.1:80`

To test the Apache server response on the internet, run this code below. An emphemeral IP was deployed for testing was

http://Public-IP-Address:80

or 

use this below: The both achieve same result.

`$ curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

![Apache-install](https://user-images.githubusercontent.com/29310552/150622412-0af38000-1366-45aa-b495-4b7770cff48d.JPG)


### Step 2: Installing Mysql
***

To install on linux system, need to run the apt command. So for this also we would run the following snippet of code

`$ sudo apt install mysql-server`

This is validated by inputing a system of password from low to high depending on our choice

![Pass-word valaidation process 3](https://user-images.githubusercontent.com/29310552/150622912-98057aba-e7c8-439c-a20f-c269251298dd.JPG)

`$ sudo mysql_secure_installation`

![mysql-nstallation](https://user-images.githubusercontent.com/29310552/150622763-e68b6c6a-e06d-40e4-b5de-75bf9735aa9c.JPG)

### Step 3: Installing PHP
***

In the previous steps we had installed Linux OS, Apache and Mysql. In this step we would install Php and other php libraries that are needed.

`$ sudo apt install php libapache2-mod-php php-mysql`

![install php-lib](https://user-images.githubusercontent.com/29310552/150664507-db2385ec-0e5d-4738-8bde-8153877759fd.JPG)

To confirm the php version, run this command on the CLI
php -v

### Step 4: Connecting with a Virtual Host
***

Create a directory for projectlamp by running this command

`$ sudo mkdir /var/www/projectlamp`

Next, assign ownership of the directory with your current system user:

`$ sudo chown -R $USER:$USER /var/www/projectlamp`

Then, create and open a new configuration file in Apache’s using the vim editor

`$ sudo vi /etc/apache2/sites-available/projectlamp.conf`

The vim editor opens a blank page. Paste this below within the vim text editor

```xml

*<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
To save and close the file, simply follow the steps below:

- Hit the esc button on the keyboard

- Type :

- Type wq. w for write and q for quit

- Hit ENTER to save the file

Use the ls command to access the files 

`sudo ls /etc/apache2/sites-available`

Using the *a2ensite* would enable the virtual host

`sudo a2ensite projectlamp`

To disable Apache’s default website use *a2dissite* command , type:

`sudo a2dissite 000-default`

To confirm no error is within your configuration, run this command:

`sudo apache2ctl configtest`

We would now relaod the Apache system by running this code:

`sudo systemctl reload apache2`

Below is output on shell


![ubuntu work](https://user-images.githubusercontent.com/29310552/150665317-238dcdaf-8f32-48a8-8dec-1e407dbbaf0d.JPG)

Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

`$ sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

You can also access your website in your browser by public DNS name, not only by IP. Below show the site been accessed through the DNS


![LAMP configure](https://user-images.githubusercontent.com/29310552/150665540-713a31de-1cdb-4a20-bdbf-280ca3ab0fdc.JPG)

### Step 5: Enabling Php on the Site
***
With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors.

In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

The command below will allow for change as discussed earlier

`sudo vim /etc/apache2/mods-enabled/dir.conf`

`<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>`

To save and close the file, simply follow the steps below:

- Hit the esc button on the keyboard

- Type :

- Type wq. w for write and q for quit

- Hit ENTER to save the file

We would need to reload Apache for the system to effect the changes

`$ sudo systemctl reload apache2`

we will create a PHP script to test that PHP is correctly installed and configured on your server.

By running this command, we could create index.php script

`$ vim /var/www/projectlamp/index.php`

![creating a php scri t](https://user-images.githubusercontent.com/29310552/150665921-b860deca-cd3b-412a-b966-c1edab413659.JPG)

To save and close the file, simply follow the steps below:

- Hit the esc button on the keyboard

- Type :

- Type wq. w for write and q for quit

- Hit ENTER to save the file

Reload to refresh the page

![Php web page](https://user-images.githubusercontent.com/29310552/150665958-6e5b51bc-8d27-4481-9c6c-9e61da64a8a2.JPG)



# This project is based on steps as outline on [Darey.io](https://www.darey.io/)








