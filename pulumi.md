# [pulumi](https://www.pulumi.com/)

## links
* [installation](https://www.pulumi.com/docs/get-started/install/)
* [get started](https://www.pulumi.com/docs/get-started/)
* [api index](https://www.pulumi.com/registry/packages/aws/api-docs/)

## pulumi python 
```sh
pip3 install pulumi
pip3 install pulumi_aws
```

## [self-managed backend](https://www.pulumi.com/docs/intro/concepts/state/#using-a-self-managed-backend)
```sh
pulumi login --local
pulumi login file:///app/data

pulumi login s3://<bucket-name>
```
### check local backend
```sh
vim ~/.pulumi/credentials.json
```

## pulumi configuration
```sh
pulumi config
pulumi config set aws:region us-east-1
```

## create project in empty folder
```sh
pulumi new aws-python
```
>  creating virtual environment by pulumi can lead to error with `apt install ... `

### environment settings
```sh
export AWS_ACCESS_KEY_ID=`aws configure get aws_access_key_id`
export AWS_SECRET_ACCESS_KEY=`aws configure get aws_secret_access_key`
export AWS_SESSION_TOKEN=`aws configure get aws_session_token`
# todo: maybe pulumi doesn't work with AWS_SESSION_TOKEN
```

### [write your code in __main__.py](https://www.pulumi.com/registry/packages/aws/api-docs/ec2/vpc/)
```sh
pulumi up
```

