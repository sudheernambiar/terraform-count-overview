# terraform-count-overview
Terraform count is a resource meta arguement, simple example

## Prerequisites
- Script is for AWS environment.
- An EC2 instance with role of admin. 
- Terraform installed.
- Need a terraform project directory with all necessary contents in it. 

### Provider.tf
```
#vim provider.tf

provider "aws" {
  region = "ap-south-1"
}
```
## Count Explained
So whenever there is a need for multiple replications or multiplication of same resource we need to deploy we can use the count meta arguement. This helps us to create multiple instaces with a single additional line of "count = <value>"
  
### Overview

### instances.tf

```
resource "aws_instance" "server" {
  <span style="color: red;">text</span>
  count = 2
  ami                    = "ami-03fa4afc89e4a8a09"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [ "sg-0f29de1098cac414f" ]
  key_name               = "Devops-TEST"
  tags = {
    "Name" = "Server"
  }
}

output "Server1_publIP" {
  value = aws_instance.server[0].public_ip
}

output "Server2_pubIP" {
  value = aws_instance.server[1].public_ip
}
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
```


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
