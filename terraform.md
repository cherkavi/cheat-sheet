# Links
* [registry](https://registry.terraform.io/)
* [documentation](https://www.terraform.io/docs/index.html)
* [configuration language](https://www.terraform.io/docs/configuration/index.html)
* [cli commands](https://www.terraform.io/docs/commands/index.html)
* [providers](https://www.terraform.io/docs/providers/)
* [cloud providers](https://www.terraform.io/docs/providers/type/major-index.html)  
* [terraform core](https://github.com/hashicorp/terraform)
* [terraform providers](https://github.com/hashicorp/terraform-providers)
* [terraform examples](https://www.terraform.io/intro/examples/index.html)


Workflow
![workflow](https://i.postimg.cc/qvXLs2D1/terraform-workflow.png)
* workflow
  * [manage different versions of terraform at one place](https://github.com/tfutils/tfenv)
* valid
  * [check errors](https://github.com/terraform-linters/tflint)
* state
  * [create from aws](https://github.com/dtan4/terraforming)
* src
  * [create dependency graph](https://github.com/28mm/blast-radius)
* plan
  * [pretty print for output plan](https://github.com/coinbase/terraform-landscape)

![commands](https://i.postimg.cc/RZ8khXTJ/terraform-commands.png)
  
```sh
# download plugins into folder .terraform
terraform init 

# set logging level: TRACE, INFO, WARN, ERROR
export TF_LOG="DEBUG"

# dry run
terraform plan
terraform plan -out will-by-applied.zip

# apply configuration
terraform apply
terraform apply -auto-approve
terraform apply will-by-applied.zip

# apply state from another file 
# skip warning: cannot import state with lineage
terraform state push -force terraform.tfstate

# remove all resources
terraform destroy


# visualisation for resources
# sudo apt install graphviz
terraform graph | dot -Tpng > out.png

# list of providers
terraform providers

# validation of current source code
terraform validate

# show resources
# get all variables for using in resources
terraform show

terraform console
# docker_image.apache.id
```
  

# cli
[list of the commands](https://www.terraform.io/docs/commands/index.html)
## [cli configuration file](https://www.terraform.io/docs/commands/cli-config.html)
```sh
cat ~/.terraformrc
```
```properties
plugin_cache_dir   = "$HOME/.terraform.d/plugin-cache"
disable_checkpoint = true
```

# [HCL configuration language](https://www.terraform.io/docs/configuration/index.html)
## [variables](https://www.terraform.io/docs/configuration/variables.html)
### input variables
usage inside the code
```json
some_resource "resource-name" {
  resource_parameter = var.p1
}
```
possible way for input variables:
* terraform.tfvars, terraform.tfvars.json
  ```json
  variable "p1" {
     default = "my own default value"
  }
  ```
* cli
  * cli var
  ```sh
  terraform apply -var 'p1=this is my parameter'
  terraform apply -var='p1=this is my parameter'  
  terraform apply -var='p1=["this","is","my","parameter"]'  
  terraform apply -var='p1={"one":"this","two":"is"}'    
  ```
  * cli var file  
  ```sh
  terraform apply -var-file="staging.tfvars"
  ```
  ```json
  p1 = "this is my param"
  p2 = [
    "one", 
    "two", 
  ]
  p3 = {
    "one": "first",
    "two": "second"
  }
  ```
* environment variables
```sh
export TF_VAR_p1="this is my parameter"
export TF_VAR_p1=["this","is","my","parameter"]  
```
### output variables
terraform code
```json
output "my_output_param" {
   value = some_resource.value.sub_param_1
}
```
terraform execution example
```sh
terraform output my_input_param
```

## workspace
![workspace](https://i.postimg.cc/mrzXt9Ld/terraform-workspaces.png)
example of usage in configuration 
```hcl
resource "aws_instance" "one_of_resources" {
  tags = {
    Name = "web - ${terraform.workspace}"
  }
}
```
```sh
terraform workspace list
terraform workspace new attempt_1
terraform workspace show
terraform workspace select default
terraform workspace select attempt_1
```
some inner mechanism
```
# workspace with name "default"
.terraform.tfstate
.terraform.tfstate.backup
./terraform.tfstate.d

# workspace with name "attempt_1"
./terraform.tfstate.d/attempt_1
./terraform.tfstate.d/attempt_1/terraform.tfstate.backup
./terraform.tfstate.d/attempt_1/terraform.tfstate

# workspace with name "attempt_2" - has not applied yet
./terraform.tfstate.d/attempt_2
``` 

# backend
Holding information about
* current state  
* configuration

[configuration example](https://medium.com/faun/terraform-remote-backend-demystified-cb4132b95057)
```json
terraform {  
    backend "s3" {
        bucket  = "aws-terraform-remote-store"
        encrypt = true
        key     = "terraform.tfstate"    
        region  = "eu-west-1"  
    }
}
```
