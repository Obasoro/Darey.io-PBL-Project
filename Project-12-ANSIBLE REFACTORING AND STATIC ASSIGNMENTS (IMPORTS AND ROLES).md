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







