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














