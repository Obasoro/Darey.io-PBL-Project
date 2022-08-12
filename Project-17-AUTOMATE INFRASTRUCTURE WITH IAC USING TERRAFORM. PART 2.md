# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM. PART 2

## A little bit more about Tagging

Tagging is a straightforward, but a very powerful concept that helps you manage your resources much more efficiently:
<strong>Note</strong>:  in out terraform.tfvars file we can have default tags defined.

```
tags = {
  Enviroment      = "production" 
  Owner-Email     = "dare@darey.io"
  Managed-By      = "Terraform"
  Billing-Account = "1234567890"
}

```

Now you can tag all you resources using the format below

```
tags = merge(
    var.tags,
    {
      Name = format("%s-PublicSubnet-%s", var.name, count.index)
    },
  )
  
```
<strong>Note</strong>: Update the variables.tf to declare the variable tags used in the format above;

```
variable "tags" {
  description = "A mapping of tags to assign to all resources."
  type        = map(string)
  default     = {}
}

```


```
variable "tags" {
  description = "A mapping of tags to assign to all resources."
  type        = map(string)
  default     = {}
}
```
