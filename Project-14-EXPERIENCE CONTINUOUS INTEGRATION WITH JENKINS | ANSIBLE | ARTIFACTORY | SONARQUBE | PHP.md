# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP

Understanding the workflow of CI?CD is important in this project 14.

# SIMULATING A TYPICAL CI/CD PIPELINE FOR A PHP BASED APPLICATION

This is part of the ongoing project that started in project 11. The idea is to have CI/CD process on the go. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both Tooling and TODO Web Applications are based on an interpreted (scripting) language (PHP)

![image](https://user-images.githubusercontent.com/29310552/175797812-62bc266a-5230-472c-8b87-04032e853ea8.png)

# Start Project Workflow

You can create a new Jenkin-Ansible server or continue with the same as you. You also adjust your ansible <strong>Inventory folder</strong> and other related folders within the project. The project setup follows make neccessary adjustement to environment we want to us.

- Ci
- Dev
- Pentest

Both <strong>SIT</strong> – For System Integration Testing and <strongUAT – User Acceptance Testing do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But Pentest – For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for Performance and Load testing. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

We would be using reverse proxy for 

![image](https://user-images.githubusercontent.com/29310552/175798193-fe6b3983-dfba-4bed-9491-0e28133f3054.png)

## CI-Environment

![image](https://user-images.githubusercontent.com/29310552/175798205-b031b845-00dc-4de8-bb09-b3b944e5c94f.png)

## Other Environments from Lower To Higher

![image](https://user-images.githubusercontent.com/29310552/175798218-8bec25c9-721a-4e88-a7f6-ed91d98fa109.png)


### Ansible Inventory folder

```
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat
```

## CI inventory file

```
[jenkins]
<Jenkins-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[sonarqube]
<SonarQube-Private-IP-Address>

[artifact_repository]
<Artifact_repository-Private-IP-Address>

```

## pentest inventory file

```
[pentest:children]
pentest-todo
pentest-tooling

[pentest-todo]
<Pentest-for-Todo-Private-IP-Address>

[pentest-tooling]
<Pentest-for-Tooling-Private-IP-Address>
```

# ANSIBLE ROLES FOR CI ENVIRONMENT

Now go ahead and Add two more roles to ansible:

- <strong>SonarQube</strong>
- <strong>Artifactory</strong>

### Configuring Ansible For Jenkins Deployment

![image](https://user-images.githubusercontent.com/29310552/175798413-628698ef-0fb7-44a1-830d-6557212468d8.png)

In the previous projects we used the command line interface (CLI), but in this project we would be running Ansible from Jenkin GUI. To achieve this

Navigate to Jenkins URL

Install & Open Blue Ocean Jenkins Plugin

Create a new pipeline

Click on manage jenkins button on the lefthand panel, click available and search for update, you can also check for clobal security within jenkins configuration to tick 

![image](https://user-images.githubusercontent.com/29310552/175799347-33ebc9fd-c660-4e60-9eaa-b91778603905.png)


![image](https://user-images.githubusercontent.com/29310552/175799294-2255c323-304a-49c5-832c-49e8ee5bce58.png)

![image](https://user-images.githubusercontent.com/29310552/175799423-64f42a83-2278-4d56-825f-6fb9fca91e0c.png)

- Navigate to Jenkins URL
- Create a new pipeline
- Select GitHub
- Connect Jenkins with GitHub
- Login to GitHub & Generate an Access Token
- Copy Access Token
- Paste the token and connect
- Create a new Pipeline
# Let us create our Jenkinsfile
Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.

![image](https://user-images.githubusercontent.com/29310552/175964863-8e05fe34-8f4c-4b47-839e-40367150b0d5.png)

```
pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}

```
![image](https://user-images.githubusercontent.com/29310552/176061140-9881f704-5890-460f-94c0-d5d9c0cb3f86.png)

![image](https://user-images.githubusercontent.com/29310552/176061229-e5a75dde-1fde-45f8-aa79-2ceabf41f23c.png)

Let us see this in action.

- Create a new git branch and name it feature/jenkinspipeline-stages
Currently we only have the Build stage. Let us add another stage called Test. 

```
pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}

```

![image](https://user-images.githubusercontent.com/29310552/176061858-f96f5157-da16-4c88-ba6a-f5dc93d5660e.png)

![image](https://user-images.githubusercontent.com/29310552/176061926-0a824d5a-039d-417e-b4ea-770e1f3a71ba.png)

# Building the Pipeline
```
1. Create a pull request to merge the latest code into the main branch
2. After merging the PR, go back into your terminal and switch into the main branch.
3. Pull the latest change.
4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
   1. Package 
   2. Deploy 
   3. Clean up
5. Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch
6. Eventually, your main branch should have a successful pipeline like this in blue ocean
```



![image](https://user-images.githubusercontent.com/29310552/176170664-6911d4cb-44a5-459b-b2f3-8f1990869756.png)

# RUNNING ANSIBLE PLAYBOOK FROM JENKINS

Installing Ansible on Jenkins
Installing Ansible plugin in Jenkins UI
Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)

We would install Ansible on the Jenkins UI
![image](https://user-images.githubusercontent.com/29310552/176323517-6847b04d-82a3-4825-aa10-9a18a4035572.png)

# Steps

- Goto Jenkins UI
- click Global tools configuration
- Look for Ansible and click. For the name put ansible, and for path /usr/bin/ (This can be gotten using "which ansible" on the terminal of the server
- Goto Credential on the Jenkin server.
- click add new credential. 
![image](https://user-images.githubusercontent.com/29310552/176587653-a3639fe0-8a40-4932-9551-6282467915e2.png)
- Copy and paste the private-key of server. If it ubuntu, redhat(ec2-user) or other linux distros, input the right name.
- Go back to dashboard, click pipeline syntax.
![image](https://user-images.githubusercontent.com/29310552/176588072-bf052479-2a23-450b-8b2f-b3b328844b5b.png)

![image](https://user-images.githubusercontent.com/29310552/176588675-3d27471b-b29b-433b-96af-090aba579726.png)

- Fill in the necessary option and click generate script

![image](https://user-images.githubusercontent.com/29310552/176588781-8235dc48-fd96-447c-8166-de4da99a1ad4.png)

![image](https://user-images.githubusercontent.com/29310552/177306928-2f260770-f925-46e0-8219-3a58a0d34ca1.png)

# Parameterizing Jenkinsfile For Ansible Deployment

1- Update sit inventory with new servers
```
[tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>
```

2 - Update Jenkinsfile to introduce parameterization
```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
...
```
![image](https://user-images.githubusercontent.com/29310552/177436748-9772dc6b-18f4-47c4-bc75-d4dd6865ba7f.png)

![image](https://user-images.githubusercontent.com/29310552/177447712-45ad7891-0c4a-4475-b087-aa5671e80a22.png)




# CI/CD PIPELINE FOR TODO APPLICATION

We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application.

## Phase 1 – Prepare Jenkins

- Fork the repository below into your GitHub account and clone into the Jenkins-ansible server
`https://github.com/darey-devops/php-todo.git`
- On you Jenkins server, install PHP, its dependencies and Composer tool (Feel free to do this manually at first, then update your Ansible accordingly later)
`sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}`
- Install Jenkins plugins
   - Plot plugin
   - Artifactory plugin

    We will use plot plugin to display tests reports, and code coverage information.
    The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server.

![image](https://user-images.githubusercontent.com/29310552/177446309-ad1899ee-3556-4ac8-9794-f9a106409155.png)

![image](https://user-images.githubusercontent.com/29310552/177564322-829d362b-2b5c-4400-aa56-ae32a694bc7f.png)

copy the ip of the artifactory server (http://ip:8082). username: admin and password:password

![image](https://user-images.githubusercontent.com/29310552/177566422-23410f5a-c46d-493f-b9f0-e68f04fd167c.png)

![image](https://user-images.githubusercontent.com/29310552/177570265-30100964-a069-4038-99b4-41a72a0df7f9.png)

# Phase 2 – Integrate Artifactory repository with Jenkins

- Create a dummy Jenkinsfile in the repository
- Using Blue Ocean, create a multibranch Jenkins pipeline
- On the database server, create database and user
- The ip address should be IP of the Jenkins server


```
Create database homestead;
CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';
GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';
```

![image](https://user-images.githubusercontent.com/29310552/177686081-09bac854-6205-473f-9081-7baabaf58a1a.png)

- Update the database connectivity requirements in the file .env.sample
- Update Jenkinsfile with proper pipeline configuration

```
pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/obasoro/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}

```
Notice the Prepare Dependencies section

The required file by PHP is .env so we are renaming .env.sample to .env
Composer is used by PHP to install all the dependent libraries used by the application
php artisan uses the .env file to setup the required database objects – (After successful run of this step, login to the database, run show tables and you will see the tables being created for you)
Install mysql

```
sudo apt install mysql-client

Log into database server!!!

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
change the bin address to 0.0.0.0

DB_CONNECTION=mysql
DB_PORT=3306
DB_HOST=private-ip-database-server


![image](https://user-images.githubusercontent.com/29310552/178030553-652187f0-0df6-4122-bca1-312a0b315951.png)

Notice the Prepare Dependencies section

The required file by PHP is .env so we are renaming .env.sample to .env
Composer is used by PHP to install all the dependent libraries used by the application
php artisan uses the .env file to setup the required database objects – (After successful run of this step, login to the database, run show tables and you will see the tables being created for you)

- Update the Jenkinsfile to include Unit tests step
```
stage('Execute Unit Tests') {
      steps {
             sh './vendor/bin/phpunit'
      } 
```
## Phase 3 – Code Quality Analysis

This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc

The data produced by phploc can be ploted onto graphs in Jenkins.

- Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file
```
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}
```
![image](https://user-images.githubusercontent.com/29310552/178066073-da3e5829-8eb1-4e92-b4f8-1eadd8950838.png)

## Phase 3 – Code Quality Analysis

This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc. The data produced by phploc can be ploted onto graphs in Jenkins

- Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.

```
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}

```
- Plot the data using plot Jenkins plugin.
```
stage('Plot Code Coverage Report') {
      steps {

            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Lines of Code (LOC),Comment Lines of Code (CLOC),Non-Comment Lines of Code (NCLOC),Logical Lines of Code (LLOC)                          ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'A - Lines of code', yaxis: 'Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Directories,Files,Namespaces', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'B - Structures Containers', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Average Class Length (LLOC),Average Method Length (LLOC),Average Function Length (LLOC)', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'C - Average Length', yaxis: 'Average Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Cyclomatic Complexity / Lines of Code,Cyclomatic Complexity / Number of Methods ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'D - Relative Cyclomatic Complexity', yaxis: 'Cyclomatic Complexity by Structure'      
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Classes,Abstract Classes,Concrete Classes', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'E - Types of Classes', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Methods,Non-Static Methods,Static Methods,Public Methods,Non-Public Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'F - Types of Methods', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Constants,Global Constants,Class Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'G - Types of Constants', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Test Classes,Test Methods', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'I - Testing', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Logical Lines of Code (LLOC),Classes Length (LLOC),Functions Length (LLOC),LLOC outside functions or classes ', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'AB - Code Structure by Logical Lines of Code', yaxis: 'Logical Lines of Code'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Functions,Named Functions,Anonymous Functions', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'H - Types of Functions', yaxis: 'Count'
            plot csvFileName: 'plot-396c4a6b-b573-41e5-85d8-73613b2ffffb.csv', csvSeries: [[displayTableFlag: false, exclusionValues: 'Interfaces,Traits,Classes,Methods,Functions,Constants', file: 'build/logs/phploc.csv', inclusionFlag: 'INCLUDE_BY_STRING', url: '']], group: 'phploc', numBuilds: '100', style: 'line', title: 'BB - Structure Objects', yaxis: 'Count'

      }
    }
```
![image](https://user-images.githubusercontent.com/29310552/178067248-fba78169-95cf-427a-bbdc-e1c167216319.png)

![image](https://user-images.githubusercontent.com/29310552/178068338-9b2ae7ad-7973-40f5-be53-00837b4a5a32.png)

- Bundle the application code for into an artifact (archived package) upload to Artifactory

```
stage ('Package Artifact') {
    steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
     }
    }
 ```
 
Install zip, if it is not installed `$ sudo apt install zip`

- Publish the resulted artifact into Artifactory

```
stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory-server'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "<name-of-artifact-repository>/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }

        }
```
![image](https://user-images.githubusercontent.com/29310552/178083407-bf005d48-3c86-44f2-9e82-abbabc38503d.png)

![image](https://user-images.githubusercontent.com/29310552/178084336-a53492e6-0c76-4cb5-918a-cdef0b86deb5.png)

- Deploy the application to the dev environment by launching Ansible pipeline
```
stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-project/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```
![image](https://user-images.githubusercontent.com/29310552/178085604-26b37076-7f9f-4f4d-b30e-f45ff20ca40b.png)

![image](https://user-images.githubusercontent.com/29310552/178085946-be60e2cf-0cd8-4b87-9a46-0c3a343c1b7c.png)

![image](https://user-images.githubusercontent.com/29310552/178088665-c7e65ea1-242a-488a-bc53-a764d4a8ac96.png)

![image](https://user-images.githubusercontent.com/29310552/178088675-4b720b43-7d3c-441c-a755-9f710e74741a.png)

# SONARQUBE INSTALLATION
Before we start getting hands on with SonarQube configuration, it is incredibly important to understand a few concepts.

Requirement for Sonarqube is 4G RAM

SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as The Sonar Way). Software testers and developers would normally work with project leads and architects to create custom quality gates.

```
sudo sysctl -w vm.max_map_count=262144
sudo sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

To make a permanent change, edit the file /etc/security/limits.conf and append the below

If your experiencing error in connection with sonarqube

Update your `ansible.cfg` file  with `roles_path = /home/ubuntu/Project-14/ansible-config-mgt-class/roles`

On your terminal connect `export ANSIBLE_CONFIG=/home/ubuntu/Project-14/ansible-config-mgt-class/deploy/ansible.cfg`

```
http://server_IP:9000 OR http://localhost:9000

```

![image](https://user-images.githubusercontent.com/29310552/178120156-7e91c39c-946c-45a7-93a9-09fb2d0043bf.png)

On the Jenkisn UI, got to managed jenkins and install SonarQube Scanner

![image](https://user-images.githubusercontent.com/29310552/178120358-133c3906-283d-4398-a819-57de2732f49b.png)

![image](https://user-images.githubusercontent.com/29310552/178120899-bd59253c-4806-46ef-af4b-6b084a0c0b28.png)

Go to the login page of sonarqube , generate a new password

Go to adminstration on sonarqube UI, click webhook and paste `http://{JENKINS_HOST}/sonarqube-webhook/`

![image](https://user-images.githubusercontent.com/29310552/178121102-6a09ad85-fde0-4dec-a328-5119e8bb8c20.png)

![image](https://user-images.githubusercontent.com/29310552/178121401-9918a7ac-e4d2-4487-bd04-f6ef34b73b49.png)

- Setup SonarQube scanner from Jenkins – Global Tool Configuration











































