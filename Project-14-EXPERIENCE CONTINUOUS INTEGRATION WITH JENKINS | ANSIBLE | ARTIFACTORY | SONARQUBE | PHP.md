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




