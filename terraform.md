# Links
* [documentation](https://www.terraform.io/docs/index.html)
* [configuration language](https://www.terraform.io/docs/configuration/index.html)
* [cli commands](https://www.terraform.io/docs/commands/index.html)
* [providers](https://www.terraform.io/docs/providers/)
* [cloud providers](https://www.terraform.io/docs/providers/type/major-index.html)  
* [terraform examples](https://github.com/cherkavi/terraform)


# cli
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
