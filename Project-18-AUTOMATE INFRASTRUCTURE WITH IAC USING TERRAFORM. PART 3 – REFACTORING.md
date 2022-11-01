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


- 1. Create a file and name it backend.tf.

```
# Note: The bucket name may not work for you since buckets are unique globally in AWS, so you must give it a unique name.
resource "aws_s3_bucket" "terraform_state" {
  bucket = "dev-terraform-bucket"
  # Enable versioning so we can see the full revision history of our state files
  versioning {
    enabled = true
  }
  # Enable server-side encryption by default
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```

- 2. Next, we will create a DynamoDB table to handle locks and perform consistency checks. In previous projects, locks were handled with a local file as shown in terraform.tfstate.lock.info. 

```
resource "aws_dynamodb_table" "terraform_locks" {
  name         = "terraform-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}

```
Terraform expects that both S3 bucket and DynamoDB resources are already created before we configure the backend. So, let us run terraform apply to provision resources.

- 3. Configure S3 Backend
```
terraform {
  backend "s3" {
    bucket         = "dev-terraform-bucket"
    key            = "global/s3/terraform.tfstate"
    region         = "eu-central-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

- 4. Verify the changes

- 5. Add Terraform Output

Before you run terraform apply let us add an output so that the S3 bucket Amazon Resource Names ARN and DynamoDB table name can be displayed.

```
output "s3_bucket_arn" {
  value       = aws_s3_bucket.terraform_state.arn
  description = "The ARN of the S3 bucket"
}
output "dynamodb_table_name" {
  value       = aws_dynamodb_table.terraform_locks.name
  description = "The name of the DynamoDB table"
}
```

- 6. Let us run terraform apply

# WHEN TO USE WORKSPACES OR DIRECTORY?

Security Groups refactoring with dynamic block

For repetitive blocks of code you can use dynamic blocks in Terraform.

### EC2 refactoring with Map and Lookup

Remember, every piece of work you do, always try to make it dynamic to accommodate future changes. Amazon Machine Image (AMI) is a regional service which means it is only available in the region it was created. But what if we change the region later, and want to dynamically pick up AMI IDs based on the available AMIs in that region? This is where we will introduce Map and Lookup functions.

```
variable "images" {
    type = "map"
    default = {
        us-east-1 = "image-1234"
        us-west-2 = "image-23834"
    }
}
```

```
resource "aws_instace" "web" {
    ami  = "${lookup(var.images, var.region), "ami-12323"}
}
```

### REFACTOR YOUR PROJECT USING MODULES

### COMPLETE THE TERRAFORM CONFIGURATION

![image](https://user-images.githubusercontent.com/29310552/199227112-d3f996f6-f8c5-4c78-b094-b9abac5ba875.png)

![image](https://user-images.githubusercontent.com/29310552/199227564-98988db0-416a-4736-b051-1218ea4ecebd.png)

![image](https://user-images.githubusercontent.com/29310552/199227667-59ef457d-8a4a-4f26-975c-6ea23f6e7e47.png)







