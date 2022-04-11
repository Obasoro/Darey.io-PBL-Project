# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

There are 

# INSTALL AND CONFIGURE JENKINS SERVER

## Step 1 – Install Jenkins server
 1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
 
 <img width="759" alt="1" src="https://user-images.githubusercontent.com/29310552/162644862-ad717e2b-9ce8-49e1-be92-c9df483bfc83.PNG">
  


 2. Install [JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) since Jenkins is a Java-based application
 
 Run the following command to upgrade and install java development kit
 
 `$ sudo apt update`
 
 <img width="703" alt="2 1" src="https://user-images.githubusercontent.com/29310552/162645181-6b0f872f-6240-4b9e-a187-8be260808f7d.PNG">

 
 `$ sudo apt install default-jdk-headless`
 
 <img width="837" alt="2 2" src="https://user-images.githubusercontent.com/29310552/162645188-b8afb0a8-71ad-4d0c-8582-b47e003e864b.PNG">
 
3. Install Jenkins
  
  Run the following commands
  
 ```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins

```

<img width="836" alt="3" src="https://user-images.githubusercontent.com/29310552/162645401-273e624a-298d-43c6-8d7b-7efd1600580a.PNG">

Confirm that Jenkins is running

`$ sudo systemctl status jenkins`

<img width="956" alt="3 1" src="https://user-images.githubusercontent.com/29310552/162645474-059a5496-d268-43af-978f-5eeb8f274101.PNG">

4. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group


<img width="856" alt="4" src="https://user-images.githubusercontent.com/29310552/162651464-2f360895-a118-4919-8794-79d485861d84.PNG">


<img width="801" alt="4 1" src="https://user-images.githubusercontent.com/29310552/162651421-c640f277-78f8-4354-881d-3891821cfc92.PNG">


<img width="776" alt="4 2" src="https://user-images.githubusercontent.com/29310552/162651423-323ec768-280e-4e68-b356-b546dcbe5a19.PNG">


# Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

## Step 1

1. For the github you want to apply webhook, fork the repository and click on the settings icon. On the left hand side a dropdown will appear.

2. Click on webhook and input the jenkins's external IP address and click

<img width="578" alt="5 1" src="https://user-images.githubusercontent.com/29310552/162651343-e0c088da-a863-425c-aa62-00a8f1badc74.PNG">

<img width="934" alt="5" src="https://user-images.githubusercontent.com/29310552/162651360-0f3a139e-dca9-47e3-84a4-ff53e8ec2aa3.PNG">

3. Go to Jenkins web console, click "New Item" and create a "Freestyle project"

<img width="718" alt="6" src="https://user-images.githubusercontent.com/29310552/162652027-1c69ac6e-6bd9-4824-a605-308fb818b7cf.PNG">

 Change the git type to http an clone the url repository. Paste the copied url in to the space created.
 
 <img width="279" alt="6 2" src="https://user-images.githubusercontent.com/29310552/162652889-da793f67-8d62-4482-91c3-ef10e8f9288c.PNG">


4. Click save to save the steps. Click on build icon to configure the jenkins

<img width="643" alt="7" src="https://user-images.githubusercontent.com/29310552/162652831-15eeb486-8002-4bfc-b22e-0ed042be2209.PNG">


<img width="773" alt="8" src="https://user-images.githubusercontent.com/29310552/162652835-c522de22-4ff2-4fe6-aafe-1678498279b9.PNG">

You can open the build and check in "Console Output" if it has run successfully.

If so – congratulations! WE have just made first Jenkins build!

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

5. Click "Configure" your job/project and add these two configurations Configure triggering the job from GitHub webhook:

<img width="651" alt="9" src="https://user-images.githubusercontent.com/29310552/162653348-478c0c2e-d19d-4702-991a-98e0ac4a142c.PNG">

Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

<img width="728" alt="10" src="https://user-images.githubusercontent.com/29310552/162653351-e2a85e93-904b-4f64-b91b-87db7d61e352.PNG">

<img width="515" alt="11" src="https://user-images.githubusercontent.com/29310552/162655627-3062f480-907d-4578-b4f8-4a6b141eeadd.PNG">


# CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH

Step 3 – Configure Jenkins to copy files to NFS server via SSH
Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "Publish Over SSH".

1. Install "Publish Over SSH" plugin.
On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.

On "Available" tab search for "Publish Over SSH" plugin and install it

<img width="907" alt="12" src="https://user-images.githubusercontent.com/29310552/162655587-81f74c4c-3938-4376-a90c-864cc225458e.PNG">

2.Configure the job/project to copy artifacts over to NFS server.

On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
Arbitrary name
Hostname – can be private IP address of your NFS server
Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server
Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

<img width="925" alt="13" src="https://user-images.githubusercontent.com/29310552/162657964-a6b1a7af-dea3-4c97-acf5-5dab7280d907.PNG">


<img width="747" alt="14" src="https://user-images.githubusercontent.com/29310552/162657970-d712cd39-3ca2-40f2-9070-5f7256fdc8fe.PNG">

If expressing error, pls the ownership of the file by running this command

`$ sudo chown -R ec2-user:ec2-user /mnt`

![image](https://user-images.githubusercontent.com/29310552/162661073-d62eb7b5-b9de-42cf-ac0e-157319752148.png)














