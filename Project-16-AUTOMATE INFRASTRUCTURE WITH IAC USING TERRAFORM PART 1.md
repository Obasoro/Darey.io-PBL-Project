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
Create a folder name it <strong>PBL</strong> within your CLI editor
Paste this code within it

```
provider "aws" {
  region = "eu-central-1"
}

# Create VPC
resource "aws_vpc" "main" {
  cidr_block                     = "172.16.0.0/16"
  enable_dns_support             = "true"
  enable_dns_hostnames           = "true"
  enable_classiclink             = "false"
  enable_classiclink_dns_support = "false"
}
```
![image](https://user-images.githubusercontent.com/29310552/180895047-c56eed4b-6770-4e6f-ba05-70fa8e3dbb19.png)




```
# Create public subnets1
    resource "aws_subnet" "public1" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.0.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "eu-central-1a"

}

# Create public subnet2
    resource "aws_subnet" "public2" {
    vpc_id                     = aws_vpc.main.id
    cidr_block                 = "172.16.1.0/24"
    map_public_ip_on_launch    = true
    availability_zone          = "eu-central-1b"
}
```
![image](https://user-images.githubusercontent.com/29310552/180894743-f0966ff5-1a95-4300-a657-82145da1f5f1.png)



![image](https://user-images.githubusercontent.com/29310552/180894622-97354b10-97dd-4a4e-9a7e-3a1e3f68bfea.png)


## FIXING THE PROBLEMS BY CODE REFACTORING

### Fixing The Problems By Code Refactoring

Fixing Hard Coded Values: We will introduce variables, and remove hard coding.

```
variable "region" {
        default = "eu-central-1"
    }

    variable "vpc_cidr" {
        default = "172.16.0.0/16"
    }

    variable "enable_dns_support" {
        default = "true"
    }

    variable "enable_dns_hostnames" {
        default ="true" 
    }

    variable "enable_classiclink" {
        default = "false"
    }

    variable "enable_classiclink_dns_support" {
        default = "false"
    }

    provider "aws" {
    region = var.region
    }

    # Create VPC
    resource "aws_vpc" "main" {
    cidr_block                     = var.vpc_cidr
    enable_dns_support             = var.enable_dns_support 
    enable_dns_hostnames           = var.enable_dns_support
    enable_classiclink             = var.enable_classiclink
    enable_classiclink_dns_support = var.enable_classiclink

    }
 ```

Terraform has a functionality that allows us to pull data which exposes information to us. For example, every region has Availability Zones (AZ). Different regions have from 2 to 4 Availability Zones. With over 20 geographic regions and over 70 AZs served by AWS, it is impossible to keep up with the latest information by hard coding the names of AZs

```
# Get list of availability zones
data "aws_availability_zones" "available" {
   state = "available"
}
```

```
# Get list of availability zones
data "aws_availability_zones" "available" {
    state = "available"
}
```



[darey.io](https://www.darey.io/docs/project-16-introduction/)

