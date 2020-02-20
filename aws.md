[sdk](https://aws.amazon.com/tools/)  

## [Security best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)

## Visual tools
* CloudFormation
* [CloudCraft](https://cloudcraft.co/)
* [VisualOps](https://visualops.readthedocs.io/)
* [draw.io](https://draw.io)
* []()

## [AWS cli](https://docs.aws.amazon.com/cli/latest/index.html)  
### installation of AWS cli
```sh
# installation
apt install aws
# set up user
aws configuration
# example of adding user to group 
aws iam add-user-to-group --group-name s3-full-access --user-name user-s3-bucket
```
### console command complition, console completion
```sh
pip3 install awscli
complete -C `locate aws_completer` aws
```

## Identity Access Manager
[IAM best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)  

## S3
```sh
aws s3 mb s3://my-bucket-name
aws s3 ls
aws s3 cp /path/to/file-name.with_extension s3://my-bucket-name
aws s3 ls s3://my-bucket-name
aws s3api head-object --bucket my-bucket-name --key file-name.with_extension
```

## EC2
[purchases options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html)

## ELB
[ELB troubleshooting](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-troubleshooting.html)

## Lambda
### [development tools](https://aws.amazon.com/serverless/developer-tools/)
* Apex
* [Python Zappa](https://github.com/Miserlou/Zappa)
* [AWS SAM](https://github.com/awslabs/serverless-application-model)
* [Go SPARTA](https://gosparta.io/)
* [aws-serverless-java-container](https://github.com/awslabs/aws-serverless-java-container)
* Chalice
...

#### Python Zappa
```sh
virtualenv env
source env/bin/activate
# update your settings https://github.com/Miserlou/Zappa#advanced-settings
zappa init
zappa deploy dev
zappa update dev
```

---
## documentation
* [official documentation](https://docs.aws.amazon.com/)  
* [whitepapers](https://aws.amazon.com/whitepapers)  
* [interactive documentation example](https://interactive.linuxacademy.com/diagrams/ProjectOmega2.html)   

# education 
* [free courses](https://www.aws.training/LearningLibrary?filters=language%3A1&search=&tab=digital_courses)  
* [amazon free training](https://www.aws.training/)  

## solutions
![serverless](https://i.postimg.cc/PxNrBPf9/aws-serverless.png)
![news](https://i.postimg.cc/zvSj5SxJ/aws-2019-re-invent.png)  

upcoming courses:
* https://aws.amazon.com/certification/certified-cloud-practitioner/
  * https://aws.amazon.com/dms/
  * https://aws.amazon.com/mp/
  * https://aws.amazon.com/vpc/
  * https://aws.amazon.com/compliance/shared-responsibility-model/
  * https://aws.amazon.com/cloudfront/
  * https://aws.amazon.com/iam/details/mfa/
  * http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html
  * http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html
  * https://aws.amazon.com/aup/
