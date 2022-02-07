# terraform-count-overview
Terraform count is a resource meta arguement, simple example

## Prerequisites
- Script is for AWS environment.
- An EC2 instance with role of admin. 
- Terraform installed.
- Need a terraform project directory with all necessary contents in it. 

## Count Explained

So whenever there is a need for multiple replications or multiplication of same resource we need to deploy we can use the count meta arguement. This helps us to create multiple instaces with a single additional line of "count = <value>". If no count mentioned in arguement list, it automatically takes that as a 1. If the count value given as 0, then it will consider as not to create the instance.
  
In backend the instances will be created as sub sections of the local resource name server as mentioned here. Instances will be consider as an array which starts from the 0 one below that count number. ie, if we mentioned two instances as we seen here, it will be taken as instance 0 and instance 1. Here at the output part, you can see aws_instance.server[0].public_ip it is mentioned 0th position and 1st position. If we have not mentioned the count/ count = 1, it will be seen as aws_instance.server.public_ip.

### Provider.tf
```
#vim provider.tf

provider "aws" {
  region = "ap-south-1"
}
```  
### Overview

### instances.tf

```
resource "aws_instance" "server" {
  count = 2
  ami                    = "ami-03fa4afc89e4a8a09"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [ "sg-0f29de1098cac414f" ]
  key_name               = "Devops-TEST"
  tags = {
    "Name" = "Server-{$count_index}"
  }
}

output "Server1_publIP" {
  value = aws_instance.server[0].public_ip
}

output "Server2_pubIP" {
  value = aws_instance.server[1].public_ip
}
```



```
In above we have got two instances IPs as mentioned in the output. 

## Execution steps.
- terraform init (to initialise with the provider)
```
$ terraform init 
```
- To identify the procedure pre flight results
```
$ terraform plan 
```
- Execute the plan (with a yes you can permit after an overview, or explicitly work it with "terraform apply -auto-approve".
```
$ terraform apply 
```
- Incase to deploy without a pre flight check.
```
$ terraform apply -auto-approve 
```
  
### Output

```
Changes to Outputs:
  + Server1_publIP = (known after apply)
  + Server2_pubIP  = (known after apply)
aws_instance.server[1]: Creating...
aws_instance.server[0]: Creating...
aws_instance.server[1]: Still creating... [10s elapsed]
aws_instance.server[0]: Still creating... [10s elapsed]
aws_instance.server[1]: Still creating... [20s elapsed]
aws_instance.server[0]: Still creating... [20s elapsed]
aws_instance.server[0]: Creation complete after 21s [id=i-0989a9687963e5b13]
aws_instance.server[1]: Still creating... [30s elapsed]
aws_instance.server[1]: Creation complete after 31s [id=i-0ed62be6f2978346b]
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

Server1_publIP = "3.110.186.106"
Server2_pubIP = "13.126.33.57"
Note: In case you want to make a clean-up use "terraform destroy"
```
```
$ terraform destroy -auto-approve
```
### Output
```
Plan: 0 to add, 0 to change, 2 to destroy.

Changes to Outputs:
  - Server1_publIP = "3.110.186.106" -> null
  - Server2_pubIP  = "13.126.33.57" -> null
aws_instance.server[1]: Destroying... [id=i-0ed62be6f2978346b]
aws_instance.server[0]: Destroying... [id=i-0989a9687963e5b13]
aws_instance.server[1]: Still destroying... [id=i-0ed62be6f2978346b, 10s elapsed]
aws_instance.server[0]: Still destroying... [id=i-0989a9687963e5b13, 10s elapsed]
aws_instance.server[1]: Still destroying... [id=i-0ed62be6f2978346b, 20s elapsed]
aws_instance.server[0]: Still destroying... [id=i-0989a9687963e5b13, 20s elapsed]
aws_instance.server[0]: Destruction complete after 30s
aws_instance.server[1]: Destruction complete after 30s

Destroy complete! Resources: 2 destroyed.
```

## Observations.
- When we executed 2 instanced were popped up. Which as per the count_index will have two names with server-0 and server-1.

## Summary

Codes can be much more simpler if we used such logics, which intact give us more agility in workplace.
