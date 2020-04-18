* [sdk](https://aws.amazon.com/tools/)  
* shell script ``` apt install aws-shell ```

## init variables
```
AWS_SNS_TOPIC_ARN=arn:aws:sns:eu-central-1:85153298123:gmail-your-name
AWS_KEY_PAIR=/path/to/file/key-pair.pem
AWS_PROFILE=aws-user
AWS_REGION=eu-central-1

# initialization from external script
. /home/projects/current-project/aws.sh
```

## url to cli documentation 
```sh
current_browser="google-chrome"
current_doc_topic="sns"
alias cli-doc='$current_browser "https://docs.aws.amazon.com/cli/latest/reference/${current_doc_topic}/index.html"'
```

## create policy from error output of aws-cli command:
>User is not authorized to perform
> AccessDeniedException
```
aws iam list-groups 2>&1 | /home/projects/bash-example/awk-policy-json.sh
# or just copy it
echo "when calling the ListFunctions operation: Use..." | /home/projects/bash-example/awk-policy-json.sh
```

## [policy generator](https://awspolicygen.s3.amazonaws.com/policygen.html)

## [AWS cli](https://docs.aws.amazon.com/cli/latest/index.html)  
### installation of AWS cli
```sh
# installation
apt install aws
# set up user
aws configuration
```
### profiling
```sh
vim ~/.aws/credentials
```
```
[cherkavi-user]
aws_access_key_id = AKI...
aws_secret_access_key = ur1DxNvEn...
```
using profiling
>  --region, --output, --profile 
```sh
aws s3 ls --profile $AWS_PROFILE
```

### console command completion, console completion
```sh
pip3 install awscli
complete -C `locate aws_completer` aws
```

---
## IAM - Identity Access Manager
[IAM best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)  
```sh
current_doc_topic="iam"
cli-doc
```

```sh
aws iam list-users 
# example of adding user to group 
aws iam add-user-to-group --group-name s3-full-access --user-name user-s3-bucket
```

---
## VPC
example of creating subnetwork:
```text
VPC: 172.31.0.0    
Subnetwork: 172.31.0.0/16, 172.31.0.0/26, 172.31.0.64/26
```
---
## S3
```sh
current_doc_topic='s3'
cli-doc
$current_browser https://console.aws.amazon.com/s3/home?region=$AWS_REGION
```

```sh
aws s3 mb s3://my-bucket-name
# list of all s3
aws s3 ls
# copy to s3
aws s3 cp /path/to/file-name.with_extension s3://my-bucket-name
# create folder, s3 mkdir
aws s3api put-object --bucket my-bucket-name --key foldername/
# sync folder with remote s3 folder
aws s3 sync /paht/to/some/folder s3://my-bucket-name/some/folder
# sync folder with remote s3 folder with public access
aws s3 sync /paht/to/some/folder s3://my-bucket-name/some/folder --acl public-read
# list of all objects
aws s3 ls --recursive s3://my-bucket-name 
# download file
aws s3api head-object --bucket my-bucket-name --key file-name.with_extension
```

policy
```json
{
    "Version": "2012-10-17",
    "Id": "policy-bucket-001",
    "Statement": [
        {
            "Sid": "statement-bucket-001",
            "Effect": "Allow",
            "Principal": "*", 
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]
}
```

add access to AccessControlList
```dotnetcli
aws s3api put-object-acl --bucket public-technik-bucket --key external-resources/index3.html --acl public-read
```

---
## [Athena](https://docs.aws.amazon.com/athena/latest)
### simple data  
s3://my-bucket-001/temp/
```csv
column-1,column-2,column3
1,one,first
2,two,second
3,three,third
4,four,fourth
5,five,fifth
```
### create database
```sql
CREATE DATABASE IF NOT EXISTS cherkavi_database_001 COMMENT 'csv example' LOCATION 's3://my-bucket-001/temp/';
```

### create table
```sql
CREATE EXTERNAL TABLE IF NOT EXISTS num_sequence (id int,column_name string,column_value string)
ROW FORMAT DELIMITED
      FIELDS TERMINATED BY ','
      ESCAPED BY '\\'
      LINES TERMINATED BY '\n'
    LOCATION 's3://my-bucket-001/temp/';

--- another way to create table 
CREATE EXTERNAL TABLE num_sequence2 (id int,column_name string,column_value string)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde' 
WITH SERDEPROPERTIES ("separatorChar" = ",", "escapeChar" = "\\") 
LOCATION 's3://my-bucket-001/temp/'    
```
### execute query
```sql
select * from num_sequence;
```

---
## CloudFront
```sh
Region <>---------- AvailabilityZone <>--------- EdgeLocation
```

---
## EC2
[purchases options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html)
```sh
# list ec2, ec2 list, instances list
aws ec2 describe-instances --profile $AWS_PROFILE --region $AWS_REGION --filters Name=tag-key,Values=test
```

---
## EBS
attach new volume
```sh
# list volumes
sudo lsblk
sudo fdisk -l
# describe volume from previous command - /dev/xvdf
sudo file -s /dev/xvdf
# !!! new partitions !!! format volume
# sudo mkfs -t xfs /dev/xvdf
# or # sudo mke2fs /dev/xvdf
# attach volume
sudo mkdir /external-drive
sudo mount /dev/xvdf /external-drive
```

---
## ELB
[ELB troubleshooting](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-troubleshooting.html)
```sh
# documentation 
current_doc_topic="elb"; cli-doc
```


---
## Lambda
### examples
* https://d1.awsstatic.com/whitepapers/architecture/AWS-Serverless-Applications-Lens.pdf

### API Gateway, Lambda url, remote invocation url
```sh
google-chrome https://"$AWS_REGION".console.aws.amazon.com/apigateway/main/apis?region="$AWS_REGION"
# API -> Stages
```
### Lambda function editor
enter point for created Lambdas
```sh
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/lambda/home?region="$AWS_REGION"#/functions"
```

### invoke function from CLI, lambda execution
```sh
# example of lambda execution
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name current-time \
--payload '{"key1": "value-1"}' \
output.log

# example of lambda execution with payload
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name current-time \
--payload '{"key1": "value-1"}' \
output.log


# example of asynchronic lambda execution with payload
# !!! with SNS downstream execution !!! 
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name current-time \
--invocation-type Event \
--payload '{"key1": "value-1"}' \
output.log
```
IAM->Policies->Create policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "lambda:InvokeFunction",
            "Resource": "arn:aws:lambda:*:*:function:*"
        }
    ]
}```


### [development tools](https://aws.amazon.com/serverless/developer-tools/)
* IntellijIDEA
* Apex
* [Python Zappa](https://github.com/Miserlou/Zappa)
* [AWS SAM](https://github.com/awslabs/serverless-application-model)
* [Go SPARTA](https://gosparta.io/)
* [aws-serverless-java-container](https://github.com/awslabs/aws-serverless-java-container)
* Chalice
...

#### [IntellijIDEA](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/getting-started.html)
* install plugin: AWS Toolkit, 
* right bottom corner - select Region, select Profile
    > profile must have: 
    ```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iam:ListRoleTags",
                "iam:GetPolicy",
                "iam:ListRolePolicies"
            ],
            "Resource": [
                "arn:aws:iam::*:policy/*",
                "arn:aws:iam::*:role/*"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "iam:ListRoles",
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::*:role/*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "*"
        }
    ]
}
    ```
* New->Project->AWS
* create new Python file from template ( my_aws_func.py )
```python
import json
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```
* <Shift><Shift> Create Lambda Function, specify handler: my_aws_func.lambda_handler

#### aws sam
```bash
pip3 install aws-sam-cli
```

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
## Route53
### [resource record types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html)


---
## SNS
```sh
current_doc_topic="sns"; cli-doc

### list of topics
aws sns list-topics --profile $AWS_PROFILE --region $AWS_REGION
#### open browser with sns dashboard
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/sns/v3/home?region="$AWS_REGION"#/topics"

### list of subscriptions
aws sns list-subscriptions-by-topic --profile $AWS_PROFILE --region $AWS_REGION --topic-arn {topic arn from previous command}

### send example via cli
    #--message file://message.txt
aws sns publish  --profile $AWS_PROFILE --region $AWS_REGION \
    --topic-arn "arn:aws:sns:us-west-2:123456789012:my-topic" \
    --message "hello from aws cli"

### send message via web 
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/sns/v3/home?region="$AWS_REGION"#/publish/topic/topics/"$AWS_SNS_TOPIC_ARN
```

---
## CloudWatch
```
 Metrics-----\
             +--->Events------>Alarm
  Logs-------/
+----------------------------------+
        dashboards
```

---
## Kinesis
### kinesis cli
```sh
# write record
aws kinesis put-record --stream-name my_kinesis_stream --partition_key "my_partition_key_1" --data "{'first':'1'}"
# describe stream
aws kinesis describe-stream --stream-name my_kinesis_stream 
# get records
aws kinesis get-shard-iterator --stream-name my_kinesis_stream --shard-id "shardId-000000000" --shard-iterator-type TRIM_HORIZON
aws kinesis get-records --shard-iterator 
```
### [Kinesis Data Generetor](https://awslabs.github.io/amazon-kinesis-data-generator/)

---
## CloudFormation
```
# cloudformation web
google-chrome "https://"$AWS_REGOIN".console.aws.amazon.com/cloudformation/home?region="$AWS_REGION"#"

# cloudformation designer web
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/cloudformation/designer/home?region="$AWS_REGION
```
### CloudFormation macros
![cloud formation macros](https://i.postimg.cc/BQMY5xmx/aws-cloud-formation-macros.png)

---
## [Security best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)

---
## Visual tools
* CloudFormation
* [CloudCraft](https://cloudcraft.co/)
* [VisualOps](https://visualops.readthedocs.io/)
* [draw.io](https://draw.io)


---
## DOCUMENTATION
* [official documentation](https://docs.aws.amazon.com/)  
* [whitepapers](https://aws.amazon.com/whitepapers)  
* [interactive documentation example](https://interactive.linuxacademy.com/diagrams/ProjectOmega2.html)   
* [cli ec2](https://docs.aws.amazon.com/cli/latest/reference/ec2/)
* [cli s3](https://docs.aws.amazon.com/cli/latest/reference/s3/)
* [cli sns](https://docs.aws.amazon.com/cli/latest/reference/sns/index.html)

# education 
* [free courses](https://www.aws.training/LearningLibrary?filters=language%3A1&search=&tab=digital_courses)  
* [amazon free training](https://www.aws.training/)  

## decisions
### way of building architecture
![way of building architecture](https://i.postimg.cc/bNzJNzLn/aws-architecture.png)  
### cloud advisor
![cloud-advisor](https://i.postimg.cc/28251PXK/aws-cloudadvisor.png)  
### fault tolerance, high performance
![serverless](https://i.postimg.cc/rwswxQBF/aws-fault-availability.jpg)  
### shared responsibility, users
![serverless](https://i.postimg.cc/y8GYP0Kh/aws-shared-model.png)  
### serverless
![serverless](https://i.postimg.cc/PxNrBPf9/aws-serverless.png)
### news
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
