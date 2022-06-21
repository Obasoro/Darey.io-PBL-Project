# ANSIBLE DYNAMIC ASSIGNMENTS (INCLUDE) AND COMMUNITY ROLES

<strong>Ansible</strong>: Ansible is an actively developing software project, so you are encouraged to visit Ansible Documentation for the latest updates on modules and their usage.

Last 2 projects We gained some knowledge and skills on Ansible, so you can perform configurations using playbooks, roles and imports. Now you will continue configuring your UAT servers learning and practicing new Ansible concepts and modules.

In this project we will introduce dynamic assignments by using ***include module.

Now you may be wondering, what is the difference between static and dynamic assignments?

Well, from Project 12, you can already tell that static assignments use import Ansible module. The module that enables dynamic assignments is include.

## Introducing Dynamic Assignment Into Our structure

In your https://github.com/<your-name>/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.

Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. We will instruct site.yml to include this playbook later. For now, let us keep building up the structure.

Your GitHub shall have following structure by now.

Note: Depending on what method you used in the previous project you may have or not have roles folder in your GitHub repository – if you used ansible-galaxy, then roles directory was only created on your Jenkins-Ansible server locally. It is recommended to have all the codes managed and tracked in GitHub, so you might want to recreate this structure manually in this case – it is up to you.


Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc., we will need a way to set values to variables per specific environment.

For this reason, we will now create a folder to keep each environment’s variables file. Therefore, create a new folder env-vars, then for each environment, create new YAML files which we will use to set variables.

Your layout should now look like this.
  
```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```
  
Now paste the instruction below into the env-vars.yml file.
  
<img width="730" alt="1" src="https://user-images.githubusercontent.com/29310552/173123310-be8d37a0-8f37-46ce-8207-a379904e96d2.PNG">
  
  
```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always
 ```
 Notice 3 things to notice here:

We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:
- include_role
- include_tasks
- include_vars
  
In the same version, variants of import were also introduces, such as:
import_role
import_tasks
  
We made use of a special variables { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.
We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.
  
# UPDATE SITE.YML WITH DYNAMIC ASSIGNMENTS
  
## Update site.yml with dynamic assignments

We Update site.yml file to make use of the dynamic assignment. 

site.yml should now look like this.
  
```
---
- hosts: all
- name: Include dynamic variables 
  tasks:
  import_playbook: ../static-assignments/common.yml 
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml
```
  
  
## Community Roles
  
Now it is time to create a role for MySQL database – it should install the MySQL package, create a database and configure users
  
Download Mysql Ansible Role
  
You can download [Geerlingguy.mysql]<https://galaxy.ansible.com/geerlingguy/mysql>
  
  <strong>Hint</strong>: To preserve your your GitHub in actual state after you install a new role – make a commit and push to master your ‘ansible-config-mgt’ directory. Of course you must have git installed and configured on Jenkins-Ansible server and, for more convenient work with codes, you can configure Visual Studio Code to work with this directory. In this case, you will no longer need webhook and Jenkins jobs to update your codes on Jenkins-Ansible server, so you can disable it – we will be using Jenkins later for a better purpose.
  

Inside roles directory create your new MySQL role with `ansible-galaxy install -p ./roles geerlingguy.mysql`
  
<img width="671" alt="4" src="https://user-images.githubusercontent.com/29310552/173129429-9f699c19-ff43-45b4-8a64-a560b238d063.PNG">
  
Inside roles directory create your new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql
  
`$ mv geerlingguy.mysql/ mysql`
  
<img width="525" alt="5" src="https://user-images.githubusercontent.com/29310552/173462102-b0eed573-a0cc-4244-8681-147b3c3f5346.PNG">
  
Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website.
  
```
git status
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin dynamic-assignment
  
```

```
git checkout main
  
git merge dynmaic-assignments
  
```
These command are to merge the main and branch together.
  
![image](https://user-images.githubusercontent.com/29310552/173464388-b7b43cc9-50c7-4cf7-a656-67f62dbfe0cc.png)
  
# LOAD BALANCER ROLES
  
## Load Balancer roles
We want to be able to choose which Load Balancer to use, Nginx or Apache, so we need to have two roles respectively:

### Nginx

`ansible-galaxy install -p ./roles geerlingguy.nginx`
  
`mv geerlingguy.nginx/ nginx`
 
![image](https://user-images.githubusercontent.com/29310552/173465248-415923cc-c298-4717-afd0-b90f49f35ab8.png)
  
### Apache
  
`ansible-galaxy install -p ./roles geerlingguy.apache`
  
`mv geerlingguy.apache/ apache`
  
![image](https://user-images.githubusercontent.com/29310552/173465796-aa7ea5df-fc7b-4a5a-b949-526628665abc.png)
  
Since you cannot use both Nginx and Apache load balancer, you need to add a condition to enable either one – this is where you can make use of variables.

Declare a variable in defaults/main.yml file inside the Nginx and Apache roles. Name each variables enable_nginx_lb and enable_apache_lb respectively.

Set both values to false like this enable_nginx_lb: false and enable_apache_lb: false.

Declare another variable in both roles load_balancer_is_required and set its value to false as well

Update both assignment and site.yml files respectively
  
loadbalancers.yml
```
- hosts: lb
  roles:
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }
 
 ```


  
site.yml file
  
```
- name: Loadbalancers assignment
        hosts: lb
        - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required 
```
  
![image](https://user-images.githubusercontent.com/29310552/174583446-df61e24a-dfba-4d7c-8741-36a22645b9b7.png)
  
Now you can make use of env-vars\uat.yml file to define which loadbalancer to use in UAT environment by setting respective environmental variable to true.

You will activate load balancer, and enable nginx by setting these in the respective environment’s env-vars file.

![image](https://user-images.githubusercontent.com/29310552/174584867-f7ac0d00-395a-44ef-bf2d-8c0a45a1d2b7.png)
  
Run this command on the ansivle-config-class
  
`$ ansible-playbook -i inventory/uat.yml playbooks/site.yml`
  
<img width="733" alt="9" src="https://user-images.githubusercontent.com/29310552/174692424-9e75986c-7735-43cd-84e7-e5a92b94e1f5.PNG">

  



  



