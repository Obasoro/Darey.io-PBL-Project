# Automate Infrastructure With IaC using Terraform. Part 4 – Terraform Cloud

[Terraform Cloud](https://www.terraform.io/cloudv) is a managed service that provides you with Terraform CLI to provision infrastructure, either on demand or in response to various events.

## Migrate your .tf codes to Terraform Cloud

1. Create a Terraform cloud account. Let us explore how we can migrate our codes to [Terraform](https://app.terraform.io/signup/account)
2. Create an organization on Terrafom
3. Configure a workspace

We will use version control workflow as the most common and recommended way to run Terraform commands triggered from our git repository.

Create a new repository in your GitHub and call it terraform-cloud, push your Terraform codes developed in the previous projects to the repository.

Choose version control workflow and you will be promped to connect your GitHub account to your workspace – follow the prompt and add your newly created repository to the workspace.

4. Configure variables
Terraform Cloud supports two types of variables: environment variables and Terraform variables. Either type can be marked as sensitive, which prevents them from being displayed in the Terraform Cloud web UI and makes them write-only.

Set two environment variables: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY

5. Now it is time to run our Terrafrom scripts, but in our previous project which was project 18, we talked about using Packer to build our images, and Ansible to configure the infrastructure, so for that we are going to make few changes to our our existing respository from Project 18.

Install [Packer](https://learn.hashicorp.com/tutorials/packer/get-started-install-cli)
Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

6. Run terraform plan and terraform apply from web console

