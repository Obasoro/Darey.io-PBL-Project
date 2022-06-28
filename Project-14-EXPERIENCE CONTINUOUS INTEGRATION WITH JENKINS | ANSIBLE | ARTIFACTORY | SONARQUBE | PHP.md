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









