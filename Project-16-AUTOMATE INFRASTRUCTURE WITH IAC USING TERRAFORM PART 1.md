# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1

![image](https://user-images.githubusercontent.com/29310552/180884452-300bf8ae-e643-44e7-a6db-d92da8856f2f.png)

## Prerequisites before you begin writing Terraform code

- Create an IAM user, name it terraform (ensure that the user has only programatic access to your AWS account) and grant this user AdministratorAccess permissions.
- Copy the secret access key and access key ID. Save them in a notepad temporarily.
- Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a Python SDK (boto3). You must have Python 3.6 or higher - on your workstation.
## Step 1

Create an S3 bucket to store Terraform state file. You can name it something like

![image](https://user-images.githubusercontent.com/29310552/180885835-9a16b3ac-58f1-4211-8419-75e049b4c578.png)

![image](https://user-images.githubusercontent.com/29310552/180887420-253335f9-2831-4f14-90ef-7da289cbb6fc.png)

## The secrets of writing quality Terraform code

## Best practices
- Ensure that every resource is tagged using multiple key-value pairs. You will see this in action as we go along.
- Try to write reusable code, avoid hard coding values wherever possible. (For learning purpose, we will start by hard coding, but gradually refactor our work to ``follow best practices).

## VPC | SUBNETS | SECURITY GROUPS


[darey.io](https://www.darey.io/docs/project-16-introduction/)

