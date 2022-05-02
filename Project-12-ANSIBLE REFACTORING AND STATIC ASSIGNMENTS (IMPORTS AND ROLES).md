# ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

In this project you will continue working with ansible-config-mgt repository which we created in projec 11 and make some improvements in our code. Now we need to refactor our Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook – it allows you to organize your tasks and reuse them when needed.

<strong>Code refactoring</strong> is the process of restructuring existing computer code—changing the factoring—without changing its external behavior. Refactoring is intended to improve the design, structure, and/or implementation of the software (its non-functional attributes), while preserving its functionality. Potential advantages of refactoring may include improved code readability and reduced complexity; these can improve the source code's maintainability and create a simpler, cleaner, or more expressive internal architecture or object model to improve extensibility. Another potential goal for refactoring is improved performance; software engineers face an ongoing challenge to write programs that perform faster or use less memory.[Wikipedia](https://en.wikipedia.org/wiki/Code_refactoring)

## Step 1 – Jenkins job enhancement

We would make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.

# Step 1 – Jenkins job enhancement

Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins 
project/job – we will require Copy Artifact plugin.

Open vscode IDE and connect to the bastion host

1.- Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build

![image](https://user-images.githubusercontent.com/29310552/166331484-8488e23a-c902-48fc-925b-a9fa44442447.png)


`$ sudo mkdir /home/ubuntu/ansible-config-artifact`

2.- Change permissions to this directory, so Jenkins could save files there –

`$ sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact`

<img width="465" alt="1" src="https://user-images.githubusercontent.com/29310552/164563191-775f5e33-a61d-491e-a4a8-6c797bd3b4e1.PNG">

3.- Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

<img width="650" alt="2" src="https://user-images.githubusercontent.com/29310552/164564697-2e9d1cc6-7c90-4ade-832e-cec3cbb94f23.PNG">

<img width="654" alt="2 1" src="https://user-images.githubusercontent.com/29310552/164570998-506f2c44-bc30-491b-a504-f1f032007d8e.PNG">

<img width="644" alt="2 2" src="https://user-images.githubusercontent.com/29310552/164571003-bcc6a173-be6f-4412-a037-7b41a95d8d54.PNG">

<img width="641" alt="2 3" src="https://user-images.githubusercontent.com/29310552/164571005-774f5575-f1e1-4303-b554-f19d9e2eddfb.PNG">

To check the number of build on the jenkins platform

<img width="507" alt="3" src="https://user-images.githubusercontent.com/29310552/164571345-5b2767ad-f4ac-4819-8985-07982bd78605.PNG">

# REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML


## Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.

<img width="558" alt="refra" src="https://user-images.githubusercontent.com/29310552/165647530-f3a2218c-6084-469b-81ad-52de8fe51a8b.PNG">


DevOps philosophy implies constant iterative improvement for better efficiency – refactoring is one of the techniques that can be used.

Most Ansible users learn the one-file approach first. However, breaking tasks up into different files is an excellent way to organize complex sets of tasks and reuse them.

Let see code re-use in action by importing other playbooks.

- Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

- Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

- Move common.yml file into the newly created static-assignments folder.

- Inside site.yml file, import common.yml playbook.

```
---
--- 
- 
  hosts: all
- 
  import_playbook: ../static-assignments/common.yml

```

The code above uses built in import_playbook Ansible module.

Your folder structure should look like this;

```

├── static-assignments
│   └── common.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
    
```

- Run ansible-playbook command against the dev environment

Since we have already done this in Projec 11, we can go ahead and update the ```common.yml``` to ```common-del.yml```

```

---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes
      
 ```
 
 update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers
 
 Access the parent workind directory of the inventory folder
 
 `$ /home/ubuntu/ansible-config-mgt/inventory`, copy and go into the configuration file of ansible
 
<img width="630" alt="delete" src="https://user-images.githubusercontent.com/29310552/165660081-5e35780b-2818-43ec-8dcf-db2d009403c3.PNG">

Now you have learned how to use import_playbooks module and you have a ready solution to install/delete packages on multiple servers with just one command.

# CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’

## Step 3 – Configure UAT Webservers with a role ‘Webserver’

We could write tasks to configure Web Servers in the same playbook, but it would be too messy, instead, we will use a dedicated role to make our configuration reusable

- Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UAT.

- To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.

There are two ways how you can create this folder structure:

Use an Ansible utility called ansible-galaxy inside ansible-config-mgt/roles directory (you need to create roles directory upfront)

```

mkdir roles
cd roles
ansible-galaxy init webserver

```

- Create the directory/files structure manually

<strong>Note</strong>: You can choose either way, but since you store all your codes in GitHub, it is recommended to create folders and files there rather than locally on Jenkins-Ansible server.

The entire folder structure should look like below, but if you create it manually – you can skip creating tests, files, and vars or remove them if you used ansible-galaxy

```

── webserver
    ├── README.md
    ├── defaults
    │   └── main.yml
    ├── files
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    ├── templates
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main.yml
        
 ```
 
After removing unnecessary directories and files, the roles structure should look like this

```
└── webserver
    ├── README.md
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    └── templates
```

<strong>Note</strong>: Ensure you are using ssh-agent to ssh into the Jenkins-Ansible instance just as it was done in project 11.

```
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

```



- In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.

- It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:
  - Install and configure Apache (httpd service)
  - Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
  - Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
  - Make sure httpd service is started
  
  Your main.yml may consist of following tasks:
  
```
---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present

- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present

- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/<your-name>/tooling.git
    dest: /var/www/html
    force: yes

- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/

- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started

- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent
  
```
  
  
# REFERENCE WEBSERVER ROLE
  
## Step 4 – Reference ‘Webserver’ role
  
Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml. This is where you will reference the role.
  
```
---
- hosts: uat-webservers
  roles:
     - webserver
```

  
Remember that the entry point to our ansible configuration is the site.yml file. Therefore, you need to refer your uat-webservers.yml role inside site.yml.

So, we should have this in site.yml
  
```
---
- hosts: all
- import_playbook: ../static-assignments/common.yml

- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
  
```
  
## Step 5 – Commit & Test
  
Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your Jenkins-Ansible server into /home/ubuntu/ansible-config-mgt/ directory
  
```
sudo ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yaml
```
<img width="737" alt="vs" src="https://user-images.githubusercontent.com/29310552/166331187-c8d08615-fe3e-4492-899b-683fae629db2.PNG">
  
We should be able to see both of your UAT Web servers configured and you can try to reach them from your browser:

http://Web1 or Web2-UAT-Server-Public-IP-or-Public-DNS-Name/index.php
  
<img width="945" alt="last" src="https://user-images.githubusercontent.com/29310552/166331620-cab0f69f-83d5-4b2a-bd83-11934d64bb4c.PNG">

![image](https://user-images.githubusercontent.com/29310552/166332838-3a95151c-b3fe-4334-b840-917eab448b15.png)


  
  



