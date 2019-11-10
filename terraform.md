# [Terraform](terraform.io)  
[Infrastructure as a code](https://www.terraform.io/docs/providers/type/major-index.html)  
* AWS
* Google Cloud Platform
* Microsoft Azure
* Digital Ocean
* AliCloud
* ...

# alternatives:
* AWS CloudFormation
* Ansible
* Puppet
* Chef
* ...

# Hashicorp Configuration Language ( TF extension )  
( GO lang)  

# [documentation](https://www.terraform.io/docs/index.html)

# cli
```sh
# download plugins into folder .terraform
terraform init 

# dry run
terraform plan
```

# terraform examples
[instance_type](https://aws.amazon.com/ec2/
[ami](https://eu-central-1.console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:)
physicalcores/)
```terraform
provider "aws" {
   access_key = "AKIA4"
   secret_key = "Wa5rB"
   region = "eu-central-1"
   version = "~> 2.35"
}

resource "aws_instance" "my-ubuntu-instance" {
    ami = "ami-0cc0a36f626a4fdf5"
    instance_type = "t3.micro"
}

```
