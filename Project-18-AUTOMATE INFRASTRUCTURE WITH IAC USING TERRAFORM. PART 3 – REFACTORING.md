# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 3 – REFACTORING

## Introducing Backend on S3

![image](https://user-images.githubusercontent.com/29310552/187100100-2470053b-6afa-462f-96b2-fa120da4e319.png)

The above architecture is to carried out using terraform ans was done in Project-16 and 17 respectively. 
But in this project, we would be sung modules and s3 bucket as backend configuration.

Here is our plan to Re-initialize Terraform to use S3 backend:

- Add S3 and DynamoDB resource blocks before deleting the local state file
- Update terraform block to introduce backend and locking
- Re-initialize terraform
- Delete the local tfstate file and check the one in S3 bucket
- Add outputs
- terraform apply

