# AWS cheat sheet

## Visual tools for drawing architecture
* [CloudFormation](https://aws.amazon.com/cloudformation/)
* [CloudCraft](https://cloudcraft.co/)
* [VisualOps](https://visualops.readthedocs.io/)
* [draw.io](https://draw.io)

## links

### [CodeCatalyst - build and deliver app on AWS ](https://codecatalyst.aws/explore)

### DOCUMENTATION
* [official documentation](https://docs.aws.amazon.com/)  
* [whitepapers](https://aws.amazon.com/whitepapers)
* [interactive documentation example](https://interactive.linuxacademy.com/diagrams/ProjectOmega2.html)   
* [cli ec2](https://docs.aws.amazon.com/cli/latest/reference/ec2/)
* [cli s3](https://docs.aws.amazon.com/cli/latest/reference/s3/)
* [cli sns](https://docs.aws.amazon.com/cli/latest/reference/sns/)
* [icons for plantUML](https://github.com/awslabs/aws-icons-for-plantuml)

### examples
* [architecture examples and explanations](https://serverlessland.com/)
* [ready to use Solutions](https://aws.amazon.com/solutions/)
* [aws cloud coach, instructor led sessions](https://pages.awscloud.com/GLB_TRAINCERT_AWS_Cloud_Coach.html)
* [udacity AWS DevOps course](https://github.com/cherkavi/udacity-cloud-devops/blob/main/README.md)
* [samples](https://github.com/aws-samples)
* [hand on](https://aws.amazon.com/getting-started/hands-on/)
* [AI code suggestion](https://aws.amazon.com/codewhisperer/)
* [Cloud Adoption Framework - best practices to help for going to cloud](https://aws.amazon.com/cloud-adoption-framework/)

### trainings
* [skill builder](https://explore.skillbuilder.aws/learn)
* [training materials bookshelf](https://online.vitalsource.com/home/my-library)
* [workshops aws](https://workshops.aws)
* [online trainings](https://www.aws.training/)
  * [online trainings free](https://www.aws.training/LearningLibrary?filters=language%3A1&search=&tab=digital_courses)  
  * [online trainings, current education](https://www.aws.training/Account/Transcript/Current)
  * [training confirmation/certificate](https://www.aws.training/Account/Transcript/Archived)
* [youtube videos](https://hackmd.io/@gekart)
* [certification preparation](https://aws.amazon.com/certification/certification-prep/)
* [labs](https://github.com/awslabs)
* [well Architected Labs - how to build more secure, resilient, high-performance applications](https://www.wellarchitectedlabs.com/?ref=wellarchitected-wp)

### others
* [serverless](https://serverlessland.com/)
* [Architecture](https://wa.aws.amazon.com/index.en.html)
* [podcasts](https://awsstash.com/)

## decisions
### way of building architecture
![way of building architecture](https://i.postimg.cc/bNzJNzLn/aws-architecture.png)  
### cloud advisor
![cloud-advisor](https://i.postimg.cc/28251PXK/aws-cloudadvisor.png)  
### fault tolerance, high performance
![fault tolerance, high performance](https://i.postimg.cc/rwswxQBF/aws-fault-availability.jpg)  
### shared responsibility model
![shared model](https://i.postimg.cc/y8GYP0Kh/aws-shared-model.png)  
### serverless
![serverless](https://i.postimg.cc/PxNrBPf9/aws-serverless.png)

## ARN - Amazon Resource Name
all the resources in cloud have `arn:` name

## [sdk - for diff prog.languages](https://aws.amazon.com/tools/) 
### [sdk endpoint, aws service endpoint](https://docs.aws.amazon.com/general/latest/gr/rande.html)
### sdk commands:
* low level commands
* high level commands
### sdk operations:
* blocking - synchronous
* unblocking - asynchronous

## aws CDK - Cloud Development Kit
```sh
pip search aws-cdk-lib
pip install aws-cdk-lib
```
```sh
npm search @aws-cdk
npm install aws-cdk@2.118.1
```
  
## [aws cli cloud](https://us-east-1.console.aws.amazon.com/cloudshell/home?region=us-east-1) 

## [AWS cli](https://docs.aws.amazon.com/cli/latest/index.html)  
> [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)
### [installation of AWS cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
> aws cli is a python application
```sh
# installation
sudo apt install awscli
pip install awscli
# set up user
aws configuration
```
### [use aws cli from docker image, aws cli without installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-docker.html#cliv2-docker-share-files)
```sh
docker run --rm -it  -v $(pwd):/aws  public.ecr.aws/aws-cli/aws-cli --version
# share local credentials with docker container 
docker run --rm -it  -v $(pwd):/aws  -v ~/.aws:/root/.aws public.ecr.aws/aws-cli/aws-cli command
```

### console command completion, console completion
```sh
pip3 install awscli
# complete -C `locate aws_completer` aws
complete -C aws_completer aws
```

### [aws cli config](https://docs.aws.amazon.com/cli/latest/topic/config-vars.html)
be aware about precedence:
1. Credentials from environment variables have precedence over credentials from the shared credentials and AWS CLI config file.   
   env variables: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html
2. Credentials specified in the shared credentials file have precedence over credentials in the AWS CLI config file. 
> botocore.exceptions.ProfileNotFound: The config profile (cherkavi-user) could not be found
```sh
vim ~/.aws/credentials
```
```properties
[cherkavi-user]
aws_access_key_id = AKI...
aws_secret_access_key = ur1DxNvEn...
aws_session_token = FwoG....
```
or 
```sh
aws configure set aws_session_token "Your-value" --profile cherkavi-user
# or
aws configure set cherkavi-user.aws_session_token "Your-value" 
```
check configuration: `vim ~/.aws/config`

using profiling
>  --region, --output, --profile 
```sh
aws configure list-profiles
# default profile will be used from env variable AWS_PROFILE
aws s3 ls --profile $AWS_PROFILE
```
### login: set AWS credentials via env variables
```sh
# source file_with_credentials.sh
export AWS_REGION=us-east-1
export AWS_ACCESS_KEY_ID=AKIA...
export AWS_SECRET_ACCESS_KEY=SEP6...
```
### login: set AWS credentials via config file
```sh
# aws cli version 2
aws configure set aws_access_key_id <yourAccessKey>
aws configure set aws_secret_access_key <yourSecretKey>
# aws configure set aws_session_token <yourToken>

# aws cli version 1
aws configure set ${AWS_PROFILE}.aws_access_key_id ...
aws configure set ${AWS_PROFILE}.aws_secret_access_key ...
# aws configure set ${AWS_PROFILE}.aws_session_token ...
```

### login: get AWS credentials via config file
```sh
aws configure get aws_access_key_id
```

### login: sso 
```sh
aws configure sso

# check configuration:
cat ~/.aws/config | grep sso-session
```
#### activate profile
```sh
aws sso login --sso-session $SSO-SESSION_NAME
aws sso login --profile $AWS_PROFILE_DEV
```
or
```sh
# aws configure export-credentials --profile RefDataFrame-cicd --format env
eval "$(aws configure export-credentials --profile RefDataFrame-cicd --format env)"
```


### debugging collaboration verbosity full request
> for API level debugging you should use CloudTrail
```sh
aws --debug s3 ls --profile $AWS_PROFILE
```

## init variables
### inline initialization
or put the same in separated file: `. /home/projects/current-project/aws.sh`
```sh
# export HOME_PROJECTS_GITHUB - path to the folder with cloned repos from https://github.com/cherkavi
export AWS_SNS_TOPIC_ARN=arn:aws:sns:eu-central-1:85153298123:gmail-your-name
export AWS_KEY_PAIR=/path/to/file/key-pair.pem
export AWS_PROFILE=aws-user
export AWS_REGION=eu-central-1

# aws default value for region 
export AWS_DEFAULT_REGION=eu-central-1

export current_browser="google-chrome" # current_browser=$BROWSER
export aws_service_abbr="sns"
function aws-cli-doc(){
    if [[ -z $aws_service_abbr ]]; then
        echo 'pls, specify the env var: aws_service_abbr'
        return 1
    fi
    x-www-browser "https://docs.aws.amazon.com/cli/latest/reference/${aws_service_abbr}/index.html" &
}
function aws-faq(){
    if [[ -z $aws_service_abbr ]]; then
        echo 'pls, specify the env var: aws_service_abbr'
        return 1
    fi
    x-www-browser "https://aws.amazon.com/${aws_service_abbr}/faqs/" &
}
function aws-feature(){
    if [[ -z $aws_service_abbr ]]; then
        echo 'pls, specify the env var: aws_service_abbr'
        return 1
    fi    
    x-www-browser "https://aws.amazon.com/${aws_service_abbr}/features/" &
}
function aws-console(){
    if [[ -z $aws_service_abbr ]]; then
        echo 'pls, specify the env var: aws_service_abbr'
        return 1
    fi
    x-www-browser "https://console.aws.amazon.com/${aws_service_abbr}/home?region=$AWS_REGION" &
}
```

---
## check configuration
```bash
vim ~/.aws/credentials

aws configure list
# default region will be used from env variable: AWS_REGION
aws configure get region --profile $AWS_PROFILE
aws configure get aws_access_key_id
aws configure get default.aws_access_key_id
aws configure get $AWS_PROFILE.aws_access_key_id
aws configure get $AWS_PROFILE.aws_secret_access_key
```

## url to cli documentation, faq, collection of questions, UI 
```sh
export aws_service_abbr="sns"
```

---
## resource query, jsonpath query, aws json output path, aws json xpath
```sh
aws ec2 describe-instances \
--query 'Reservations[*].Instances[*].PublicIpAddress' \
--filters "Name=tag:Project,Values=udacity"
```

## [resource sql query](https://docs.aws.amazon.com/config/latest/developerguide/query-using-sql-editor-cli.html)
```sh
aws configservice select-resource-config --expression "SELECT resourceId WHERE resourceType='AWS::EC2::Instance'"
```

## budget limit, cost management, budget cost explorer
> price/cost formation: write - free, read - payable
```sh
aws_service_abbr="cost-management"
x-www-browser https://${AWS_REGION}.console.aws.amazon.com/cost-management/home?region=${AWS_REGION}#/dashboard
x-www-browser https://${AWS_REGION}.console.aws.amazon.com/billing/home?region=${AWS_REGION}#/budgets
```
---
## IAM - Identity Access Manager
Type of the access:
* Temporary - role
* Permanently - user
![iam](https://i.ibb.co/LCHWKqc/aws-2023-08-27-aws-identity-access-management-png.jpg)
* [AWS Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
* [IAM best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
* [IAM roles - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
* [AWS Identity and Access Management Access Analyzer](https://aws.amazon.com/iam/access-analyzer/)
* [Service control policies SCP](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
* [relations between entities](#shared-responsibility-model)
```sh
aws_service_abbr="iam"
aws-cli-doc
aws-faq
aws-console
```

```sh
aws iam list-users 

# example of adding user to group 
aws iam add-user-to-group --group-name s3-full-access --user-name user-s3-bucket

# get role 
aws iam list-roles
aws iam get-role --role-name $ROLE_NAME

# policy find by name
POLICY_NAME=AmazonEKSWorkerNodePolicy
aws iam list-policies --query "Policies[?PolicyName=='$POLICY_NAME']"
aws iam list-policies --output text --query 'Policies[?PolicyName == `$POLICY_NAME`].Arn'
# policy get by ARN
aws iam get-policy-version --policy-arn $POLICY_ARN --version-id v1
# policy list 
aws iam list-attached-role-policies --role-name $ROLE_NAME
# policy attach 
aws iam attach-role-policy --policy-arn $POLICY_ARN --role-name $ROLE_NAME
```
[example of role with policy creation with awscli](https://github.com/cherkavi/udacity-aws-devops-eks/blob/main/README-for-users.md#create-codebuild-role)

## Policy
* [AWS Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)

### Policy types
* Resource based
* IAM based

### Policy parts:
* [Principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) ( User, Group )
* Action (example for s3 [keys and action](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3.html))
* Resource
* Condition  
  tag of the resource can be involved in condition 

### create policy from error output of aws-cli command:
>User is not authorized to perform
> AccessDeniedException
```sh
aws iam list-groups 2>&1 | /home/projects/bash-example/awk-policy-json.sh
# or just copy it
echo "when calling the ListFunctions operation: Use..." | /home/projects/bash-example/awk-policy-json.sh
```

### [policy generator](https://awspolicygen.s3.amazonaws.com/policygen.html)

### [policy simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)
> test sandbox for the policy


---
## VPC
```sh
aws_service_abbr="vpc"
aws-cli-doc
aws-faq
aws-console
```

example of creating subnetwork:
```text
VPC: 172.31.0.0    
Subnetwork: 172.31.0.0/16, 172.31.0.0/26, 172.31.0.64/26
```
### VPC Flow logs
like Wireshark for VPC

### VPC endpoint
it is internal tunnel betweeen VPC and the rest of AWS resources  
when you are creating target endpoint (to access S3, for instance) and want to use it from ec2, then add also  
* SSMMessagesEndpoint
* EC2MessagesEndpoint

---
## NAT 
> NAT Gateway (NGW) allows instances with no public IPs to access the internet.

---
## IGW
> Internet Gateway (IGW) allows instances with public IPs to access the internet.

public access internet outside access
1. create gateway
2 .vpc -> route tables -> add route 
Security Group
1. inbound rules -> source 0.0.0.0/0

---
## Transit Gateway
1. Create Transit Gateway
2. Create Transit Gateway attachment
3. Create Transit Gateway route table
4. Create Transit Gateway route table association
5. Create Transit Gateway route table propagation
6. Update VPC's route tables

## Storage Gateway
> is a service that connects an on-premises software appliance with cloud-based storage to provide seamless and secure integration between your on-premises IT environment and the AWS storage infrastructure in the AWS Cloud

---
## S3 - object storage 
> there are S3 Filegateway 
```sh
aws_service_abbr='s3'
aws-cli-doc
aws-faq
aws-console
```
### s3 static web site
* [S3 api](https://docs.aws.amazon.com/pdfs/AmazonS3/latest/API/s3-api.pdf)
* [static web site solution](https://github.com/cherkavi/udacity-cloud-devops/blob/main/08-static-web-site.md#static-web-site)
* [static web site doc](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)
* [static web site endpoints](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html)
* alternative solutions for making static website hosting
  * Backblaze B2
  * DigitalOcean
  * Oracle Cloud Infrastructure Object Storage
  * IBM Cloud Object Storage
  * Alibaba Cloud OSS
  * Microsoft Azure Blob Storage
  * Google Cloud Storage
* TODO: how to add WAF to S3
### s3 operations
```sh
# make bucket - create bucket with globally unique name
AWS_BUCKET_NAME="my-bucket-name" 
aws s3 mb s3://$AWS_BUCKET_NAME
aws s3 mb s3://$AWS_BUCKET_NAME --region us-east-1 

# https://docs.aws.amazon.com/cli/latest/reference/s3api/create-bucket.html
# public access - Block all public access - Off
aws s3api create-bucket --bucket $AWS_BUCKET_NAME --acl public-read-write

# enable mfa delete
aws s3api put-bucket-versioning --bucket $AWS_BUCKET_NAME --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn-of-mfa-device mfa-code" --profile root-mfa-delete-demo
# disable mfa delete
aws s3api put-bucket-versioning --bucket $AWS_BUCKET_NAME --versioning-configuration Status=Enabled,MFADelete=Disabled --mfa "arn-of-mfa-device mfa-code" --profile root-mfa-delete-demo

# list of all s3
aws s3 ls
aws s3api list-buckets
aws s3api list-buckets --query "Buckets[].Name"
aws s3api list-buckets --query 'Buckets[?contains(Name, `my_bucket_name`) == `true`] | [0].Name' --output text 

# Bucket Policy, public read ( Block all public access - Off )
aws s3api get-bucket-location --bucket $AWS_BUCKET_NAME

# put object
aws s3api put-object --bucket $AWS_BUCKET_NAME --key file-name.with_extension --body /path/to/file-name.with_extension
# copy to s3, upload file less than 5 Tb
aws s3 cp /path/to/file-name.with_extension s3://$AWS_BUCKET_NAME
aws s3 cp /path/to/file-name.with_extension s3://$AWS_BUCKET_NAME/path/on/s3/filename.ext
# update metadata
aws s3 cp test.txt s3://a-bucket/test.txt --metadata '{"x-amz-meta-cms-id":"34533452"}'
# read metadata
aws s3api head-object --bucket a-bucketbucket --key img/dir/legal-global/zach-walsh.jpeg

# copy from s3 to s3
aws s3 cp s3://$AWS_BUCKET_NAME/index.html s3://$AWS_BUCKET_NAME/index2.html

# download file
aws s3api get-object --bucket $AWS_BUCKET_NAME --key path/on/s3 /local/path

# create folder, s3 mkdir
aws s3api put-object --bucket my-bucket-name --key foldername/
# sync folder local to remote s3
aws s3 sync /path/to/some/folder s3://my-bucket-name/some/folder
# sync folder remote s3 to local
aws s3 sync s3://my-bucket-name/some/folder /path/to/some/folder 
# sync folder with remote s3 bucket with public access
aws s3 sync /path/to/some/folder s3://my-bucket-name/some/folder --acl public-read
# sync folder with remote s3 bucket and remove all not existing files locally but existing in bucket
aws s3 sync s3://my-bucket-name/some/folder /path/to/some/folder --delete
# list of all objects
aws s3 ls --recursive s3://my-bucket-name 
# list of all object by specified path ( / at the end must be )
aws s3 ls --recursive s3://my-bucket-name/my-sub-path/
# download file
aws s3api head-object --bucket my-bucket-name --key file-name.with_extension
# move file 
aws s3 mv s3://$AWS_BUCKET_NAME/index.html s3://$AWS_BUCKET_NAME/index2.html
# remove file remove object
aws s3 rm  s3://$AWS_BUCKET_NAME/file-name.with_extension 
aws s3api delete-object --bucket $AWS_BUCKET_NAME --key file-name.with_extension 
# remove all objects
aws s3 rm s3://$AWS_S3_BUCKET_NAME --recursive --exclude "account.json" --include "*"
#!!! using only '--include "test-file*"' - will remove all files, not only specified in include section !!!, use instead of:

# upload file and make it public
aws s3api put-object-acl --bucket <bucket name> --key <path to file> --acl public-read
# read file 
aws s3api get-object --bucket <bucket-name> --key=<path on s3> <local output file>

# read version of object on S3
aws s3api list-object-versions --bucket $AWS_BUCKET_NAME --prefix $FILE_KEY
# read file by version 
aws s3api get-object --bucket $AWS_S3_BUCKET_NAME --version-id $VERSION_ID --key d3a274bb1aba08ce403a6a451c0298b9 /home/projects/temp/$VERSION_ID

# history object history list
aws s3api list-object-versions --bucket $AWS_S3_BUCKET_NAME --prefix $AWS_FILE_KEY | jq '.Versions[]' | jq '[.LastModified,.Key,.VersionId] | join(" ")' | grep -v "_response" | sort | sed "s/\"//g"
```
```sh
# remove s3
aws s3 ls
aws s3 rm s3://$AWS_BUCKET_NAME --recursive --include "*"
aws s3api delete-bucket --bucket $AWS_BUCKET_NAME
```

* Bucket Policy, public read ( Block all public access - Off )
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
* Access Control List - individual objects level
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:GetObjectAcl",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::*/*"
        }
    ]
}
```
* Full access for role 
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::INSERT-ACCOUNT-NUMBER:role/INSERT_ROLE_NAME"
      },
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::INSERT-BUCKET-NAME",
        "arn:aws:s3:::INSERT-BUCKET-NAME/*"
      ]
    }
  ]
}
```

---
## datasync
>  move large amounts of data between on-premises storage and AWS storage services  
>  DataSync configuration can be set up via AWS Management Console  
```sh
aws_service_abbr="datasync"
aws-cli-doc
aws-faq
aws-console
aws-feature
```

### datasync links
* [install agent VM](https://docs.aws.amazon.com/datasync/latest/userguide/deploy-agents.html#create-vmw-agent)
* [activate endpoint](https://docs.aws.amazon.com/datasync/latest/userguide/choose-service-endpoint.html)
* [activate agent VM](https://docs.aws.amazon.com/datasync/latest/userguide/activate-agent.html)
* [workshop big migration](https://catalog.us-east-1.prod.workshops.aws/datasync-migrate-millions-of-files/en-US) 
* [workshop windows migration](https://catalog.us-east-1.prod.workshops.aws/datasync-fsx-windows-migration/en-US) 

### steps
1. download datasync agent image 
   DataSync -> Agents -> Create agent -> Deploy Agent
   1. download image
   2. start image in player 
      1. install  Oracle VM Virtual Box
        ```sh
        sudo apt install virtualbox
        ```
      2. File > Import Appliance…
      3. for entering into VM (ssh): 
         ```sh
         DATASYNC_AGENT_IP=192.168.178.48
         DATASYNC_SSH_USER=admin
         DATASYNC_SSH_PASS=password
         sshpass -p ${DATASYNC_SSH_PASS} ssh ${DATASYNC_SSH_USER}@${DATASYNC_AGENT_IP}
         ```        
2. [activate datasync agent](https://docs.aws.amazon.com/datasync/latest/userguide/activate-agent.html#get-activation-key)
   1. via ssh
   2. via curl ( select CLI )
   ```sh
   DATASYNC_AGENT_IP=192.168.178.48
   ACTIVATION_REGION=eu-central-1
   curl "http://${DATASYNC_AGENT_IP}/?gatewayType=SYNC&activationRegion=${ACTIVATION_REGION}&no_redirect"
   
   # key 5C6V2-zzzz-yyyy-xxxx-xxxx
   ```
3. register/create DataSync via [aws console](https://console.aws.amazon.com/datasync)
   DataSync -> Agents -> create agent
   enter your key 
4. [DataSync -> Tasks -> Create task](https://console.aws.amazon.com/datasync)
5. configure DataSync options.
6. Start, monitor, and review the DataSync task.

### Agent
* An agent is a virtual machine (VM) used to read data from or write data to a location. 
* An agent is used on premises(in cloud) to manage both read and write operations.
* Agent status: online/offline
  [A location](https://docs.aws.amazon.com/datasync/latest/userguide/working-with-locations.html) is a:
  * endpoint of a task
  * source
  * destination 
  * service 
  used in the data transfer task.
* [DataSync agent overview](https://docs.aws.amazon.com/datasync/latest/userguide/managing-datasync.html)
  1. TCP: 80 (HTTP)
     Used by your computer to obtain the agent activation key. After successful activation, DataSync closes the agent's port 80.  
  2. TCP: 443 (HTTPS)
     * Used by the DataSync agent to activate with your AWS account. This is for agent activation only. You can block the endpoints after activation.
     * For communication between the DataSync agent and the AWS service endpoint.
     API endpoints: datasync.$region.amazonaws.com  
     Data transfer endpoints: $taskId.datasync-dp.$region.amazonaws.com cp.datasync.$region.amazonaws.com  
     Data transfer endpoints for FIPS: cp.datasync-fips.$region.amazonaws.com  
     Agent updates: repo.$region.amazonaws.com repo.default.amazonaws.com packages.$region.amazonaws.com  
  3. TCP/UDP: 53 (DNS)
     For communication between DataSync agent and the DNS server.
  4. TCP: 22
     Allows AWS Support to access your DataSync to help you with troubleshooting DataSync issues. You don't need this port open for normal operation, but it is required for troubleshooting.
  5. UDP: 123 (NTP)
     Used by local systems to synchronize VM time to the host time.
     NTP
     0.amazon.pool.ntp.org
     1.amazon.pool.ntp.org
     2.amazon.pool.ntp.org
     3.amazon.pool.ntp.org                                 

### Task 
[Task management/configuration](https://docs.aws.amazon.com/datasync/latest/userguide/create-task-how-to.html) can be set up via:
* AWS Management Console
* AWS Command Line Interface (AWS CLI)
* DataSync API 
Task configuration settings includes:
* source 
* destination 
* options
* filtering (include/exclude)
* scheduling 
* frequency
* tags
* logging
Task phases
* queuing 
* launching
* preparing
* transferring
* verifying (VerifyMode)
* success/failure

---
## RDS
> should be considered DataStorage type like ( see CommandQueryResponsibilitySegregation ):  
> * read heavy  
> * write heavy  
> [jdbc wrapper](https://github.com/aws/aws-advanced-jdbc-wrapper)  
> there are "Database Migration Service"

### PostgreSQL
!!! important during creation need to set up next parameter:  
Additional configuration->Database options->Initial Database -> <name of your schema>  
default schema - postgres
!!! if you have created Public accessible DB, pls, check/create inbound rule in security group:
IPv4	PostgreSQL	TCP	5432	0.0.0.0/0

---
## [Athena](https://docs.aws.amazon.com/athena/latest)
```sh
aws_service_abbr="athena"
aws-cli-doc
aws-faq
aws-console
```

```
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
![cloud-front](https://i.ibb.co/jHLb1Vn/aws-2023-08-27-aws-cloud-front-decreased-png.jpg)  

```sh
aws_service_abbr="cloudfront"
aws-cli-doc
aws-faq
aws-console
```

```mermaid
flowchart RL;
    AvailabilityZone --o Region
        EdgeLocation --o Region
```
> nice to have 2-3 availability zones
```sh
REGION=us-east-1
BUCKET_NAME=bucket-for-static-web
BUCKET_HOST=$BUCKET_NAME.s3-website-$REGION.amazonaws.com
DISTRIBUTION_ID=$BUCKET_HOST'-cli-3'
DOMAIN_NAME=$BUCKET_HOST
echo '{
    "CallerReference": "cli-example",
    "Aliases": {
        "Quantity": 0
    },
    "DefaultRootObject": "index.html",
    "Origins": {
        "Quantity": 1,
        "Items": [
            {
                "Id": "'$DISTRIBUTION_ID'",
                "DomainName": "'$DOMAIN_NAME'",
                "OriginPath": "",
                "CustomHeaders": {
                    "Quantity": 0
                },
                "CustomOriginConfig": {
                    "HTTPPort": 80,
                    "HTTPSPort": 443,
                    "OriginProtocolPolicy": "http-only",
                    "OriginSslProtocols": {
                        "Quantity": 1,
                        "Items": [
                            "TLSv1.2"
                        ]
                    },
                    "OriginReadTimeout": 30,
                    "OriginKeepaliveTimeout": 5
                },
                "ConnectionAttempts": 3,
                "ConnectionTimeout": 10,
                "OriginShield": {
                    "Enabled": false
                },
                "OriginAccessControlId": ""
            }
        ]
    },
    "OriginGroups": {
        "Quantity": 0
    },
    "DefaultCacheBehavior": {
        "TargetOriginId": "'$DISTRIBUTION_ID'",
        "ForwardedValues": {
            "QueryString": false,
            "Cookies": {
                "Forward": "none"
            },
            "Headers": {
                "Quantity": 0
            },
            "QueryStringCacheKeys": {
                "Quantity": 0
            }
        },        
        "TrustedSigners": {
            "Enabled": false,
            "Quantity": 0
        },
        "TrustedKeyGroups": {
            "Enabled": false,
            "Quantity": 0
        },
        "ViewerProtocolPolicy": "redirect-to-https",
        "MinTTL": 0,
        "AllowedMethods": {
            "Quantity": 2,
            "Items": [
                "HEAD",
                "GET"
            ],
            "CachedMethods": {
                "Quantity": 2,
                "Items": [
                    "HEAD",
                    "GET"
                ]
            }
        },
        "SmoothStreaming": false,
        "Compress": true,
        "LambdaFunctionAssociations": {
            "Quantity": 0
        },
        "FunctionAssociations": {
            "Quantity": 0
        },
        "FieldLevelEncryptionId": ""
    },
    "CacheBehaviors": {
        "Quantity": 0
    },
    "CustomErrorResponses": {
        "Quantity": 0
    },
    "Comment": "",
    "PriceClass": "PriceClass_All",
    "Enabled": true,
    "ViewerCertificate": {
        "CloudFrontDefaultCertificate": true,
        "SSLSupportMethod": "vip",
        "MinimumProtocolVersion": "TLSv1",
        "CertificateSource": "cloudfront"
    },
    "Restrictions": {
        "GeoRestriction": {
            "RestrictionType": "none",
            "Quantity": 0
        }
    },
    "WebACLId": "",
    "HttpVersion": "http2",
    "IsIPV6Enabled": true,
    "Staging": false
}' > distribution-config.json
# vim distribution-config.json
aws cloudfront create-distribution --distribution-config file://distribution-config.json
# "ETag": "E2ADZ1SMWE",   

aws cloudfront list-distributions | grep DomainName

# aws cloudfront list-distributions | grep '"Id":'
# aws cloudfront delete-distribution --id E6Q0X5NZY --if-match E2ADZ1SMWE
```

```sh
### cloudfront delete
DISTRIBUTION_ID=`aws cloudfront list-distributions | jq -r ".DistributionList.Items[].Id"`
echo $DISTRIBUTION_ID | clipboard
aws cloudfront get-distribution --id $DISTRIBUTION_ID > $DISTRIBUTION_ID.cloud_front
DISTRIBUTION_ETAG=`jq -r .ETag $DISTRIBUTION_ID.cloud_front`
## disable distribution
# fx $DISTRIBUTION_ID.cloud_front
jq '.Distribution.DistributionConfig.Enabled = false' $DISTRIBUTION_ID.cloud_front |  jq '.Distribution.DistributionConfig' > $DISTRIBUTION_ID.cloud_front_updated 
aws cloudfront update-distribution --id $DISTRIBUTION_ID --if-match $DISTRIBUTION_ETAG --distribution-config file://$DISTRIBUTION_ID.cloud_front_updated 
## remove distribution
aws cloudfront get-distribution --id $DISTRIBUTION_ID > $DISTRIBUTION_ID.cloud_front
DISTRIBUTION_ETAG=`jq -r .ETag $DISTRIBUTION_ID.cloud_front`
aws cloudfront delete-distribution --id $DISTRIBUTION_ID --if-match $DISTRIBUTION_ETAG

```

---
## Secrets manager
```sh
aws_service_abbr="secretsmanager"
aws-cli-doc
aws-faq
aws-console
```
[boto3 python lib](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/secretsmanager.html#SecretsManager.Client.describe_secret)  
see:  
* [aws config tool](https://aws.amazon.com/config/)
  > autoupdate changes 
  > sends SNS onChange
* [aws parameters store](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-store-working-with.html)

```sh
### CLI example
# read secret
aws secretsmanager get-secret-value --secret-id LinkedIn_project_Web_LLC --region $AWS_REGION --profile cherkavi-user
```
readonly policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:*:*:secret:*"
        }
    ]
}
```
```sh
# create secret
aws secretsmanager put-secret-value --secret-id MyTestDatabaseSecret --secret-string file://mycreds.json

# create secret for DB
aws secretsmanager create-secret \
    --name $DB_SECRET_NAME \
    --secret-string "{\"engine\":\"mysql\",\"username\":\"$DB_LOGIN\",\"password\":\"$DB_PASSWORD\",\"dbname\":\"$DB_NAME\",\"port\": \"3306\",\"host\": $DB_ADDRESS}"
```

---
## EC2
```sh
aws_service_abbr="ec2"
aws-cli-doc
aws-faq
aws-console
```
    
[purchases options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html)
```sh
# list ec2, ec2 list, instances list
aws ec2 describe-instances --profile $AWS_PROFILE --region $AWS_REGION --filters Name=tag-key,Values=test
# example
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:Name,Values=ApplicationInstance"
# !!! without --filters will give you not a full list of EC2 !!!
```
connect to launched instance
```sh
INSTANCE_PUBLIC_DNS="ec2-52-29-176.eu-central-1.compute.amazonaws.com"
ssh -i $AWS_KEY_PAIR ubuntu@$INSTANCE_PUBLIC_DNS
```

connect to instance in private subnet, bastion approach
```mermaid
flowchart LR;
    a[actor] -->|inventory| jb
    subgraph public subnet    
        jb[ec2 
           jumpbox]
    end
    subgraph private subnet    
        s[ec2 
          server]
    end
    jb -->|inventory| s

```

reading information about current instance, local ip address, my ip address, connection to current instance, instance reflection, instance metadata, instance description
```sh
curl http://169.254.169.254/latest/meta-data/
curl http://169.254.169.254/latest/meta-data/instance-id
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
curl http://169.254.169.254/latest/api/token
# public ip 
curl http://169.254.169.254/latest/meta-data/public-ipv4

curl http://169.254.169.254/latest/dynamic/instance-identity/document
```

create token from ec2 instance
```sh
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
export AWS_REGION=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
```

connect to launched instance without ssh
> using SSM-agent installed in 
```sh
# ssm role should be provided for account
aws ssm start-session --target i-00ac7eee --profile awsstudent --region us-east-1
```

DNS issue space exceed
```sh
sudo systemctl restart systemd-resolved
sudo vim /etc/resolv.conf
```
```text
# nameserver 127.0.0.53
nameserver 10.0.0.2
options edns0 trust-ad
search ec2.internal
```

---
## SSM
```sh
aws_service_abbr="ssm"
aws-cli-doc
aws-faq
```
```sh
# GET PARAMETERS
aws ssm get-parameters --names /my-app/dev/db-url /my-app/dev/db-password
aws ssm get-parameters --names /my-app/dev/db-url /my-app/dev/db-password --with-decryption

# GET PARAMETERS BY PATH
aws ssm get-parameters-by-path --path /my-app/dev/
aws ssm get-parameters-by-path --path /my-app/ --recursive
aws ssm get-parameters-by-path --path /my-app/ --recursive --with-decryption
```

---
## EBS - block storage
```sh
aws_service_abbr="ebs"
aws-cli-doc
aws-faq
```

> snapshot can be created from one ESB
> snapshot can be copied to another region
> volume can be created from snapshot and attached to EC2
> ESB --> Snapshot --> copy to region --> Snapshot --> ESB --> attach to EC2

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
## ELB - Elastic Load Balancer
![elb](https://i.ibb.co/1JPnBHL/aws-2023-08-27-aws-elb-autoscaling-png.jpg)  
```sh
aws_service_abbr="elb"
aws-cli-doc
aws-faq
```

```mermaid
flowchart LR;
    r[Request] --> lb
    l[Listener] --o lb[Load Balancer]

    lr[Listener
       Rule] --o l
    lr --> target_group1
    lr --> target_group2
    lr --> target_group3

    subgraph target_group1
        c11[ec2]
        c12[ec2]
        c13[ec2]
    end
    subgraph target_group2
        c21[ec2]
        c22[ec2]
        c23[ec2]
    end
    subgraph target_group3
        c31[ec2]
        c32[ec2]
        c33[ec2]
    end
    
```
![autoscaling group](https://github.com/cherkavi/cheat-sheet/assets/8113355/c2688e3f-ded4-4ca6-99a6-b5fe1ba4d34a)


[ELB troubleshooting](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-troubleshooting.html)
```sh
# documentation 
aws_service_abbr="elb"; cli-doc
```

---
## EFS - Elastic File System storage
```sh
aws_service_abbr="efs"
aws-cli-doc
aws-faq
```
```sh
# how to write files into /efs and they'll be available on both your ec2 instances!

# on both instances:
sudo yum install -y amazon-efs-utils
sudo mkdir /efs
sudo mount -t efs fs-yourid:/ /efs
```

---
## SQS - Queue Service
> types: standart, fifo
```sh
aws_service_abbr="sqs"
aws-cli-doc
aws-faq
```
```sh
# get CLI help
aws sqs help

# list queues and specify the region
aws sqs list-queues --region $AWS_REGION

AWS_QUEUE_URL=https://queue.amazonaws.com/3877777777/MyQueue 
```

```sh
aws sqs send-message --queue-url ${QUEUE_NAME} --message-body '{"test":00001}'
# status of the message - available 
aws sqs receive-message --queue-url ${QUEUE_NAME}
# status of the message - message in flight
RECEIPT_HANDLE=$(echo $RECEIVE_MESSAGE_OUTPUT | jq -r '.Messages[0].ReceiptHandle')
aws sqs delete-message --queue-url ${QUEUE_NAME} --receipt-handle $RECEIPT_HANDLE
# status of the message - not available 
```

```sh
# send a message
aws sqs send-message help
aws sqs send-message --queue-url $AWS_QUEUE_URL --region $AWS_REGION --message-body "my test message"

# receive a message
aws sqs receive-message help
aws sqs receive-message --region $AWS_REGION  --queue-url $AWS_QUEUE_URL --max-number-of-messages 10 --visibility-timeout 30 --wait-time-seconds 20

# delete a message ( confirmation of receiving !!! )
aws sqs delete-message help
aws sqs receive-message --region us-east-1  --queue-url $AWS_QUEUE_URL --max-number-of-messages 10 --visibility-timeout 30 --wait-time-seconds 20

aws sqs delete-message --receipt-handle $MESSAGE_ID1 $MESSAGE_ID2 $MESSAGE_ID3 --queue-url $AWS_QUEUE_URL --region $AWS_REGION
```

---
## EventBridge
> Event hub that receives, collects, filters, routes events ( message with body and head ) based on rules     
> to receiver back, to another serviceS, to apiS ...     
> Similar to SQS but wider.   
> Offers comprehensive monitoring and auditing capabilities.    

### terms
* Event  
  A JSON-formatted message that represents a change in state or occurrence in an application or system
* Event bus
  A pipeline that receives events from various sources and routes them to targets based on defined rules
* Event source
  The origin of events, which can be AWS services, custom applications, or third-party SaaS providers
* Event pattern
  A JSON-based structure that is used in rules to define criteria for matching events
* Schema
  A structured definition of an event's format, which can be used for code generation and validation
* Rule
  Criteria that are used to match incoming events and determine how they should be processed or routed
* Archive
  A feature that makes it possible for you to store events for later analysis or replay
* Target
  The destination where matched events are sent, which offers options for event transformation, further processing, and reliable delivery mechanisms, including dead-letter queues

### links
* https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html
* https://aws.amazon.com/blogs/compute/category/application-integration/amazon-eventbridge/

---
## Lambda
* java - best choice ( price, exec.time ), GraalVM?
* python - 35x time slower!!! than java
* rust - not for business applications
* tradeoff: lambda vs k8s(EC2)
```sh
aws_service_abbr="lambda"
aws-cli-doc
aws-faq
aws-console
```
> `/tmp` directory can be used for saving cache or collaboration between multiple invocations

### list of functions
```sh
aws lambda list-functions --function-version ALL --region us-east-1 --output text --query "Functions[?Runtime=='python3.7'].FunctionArn"
```

### remove function
```sh
# aws lambda update-function-configuration --function-name my-function --environment "Variables={FIRST=1,SECOND=2}"
# aws lambda list-functions --query 'Functions[].FunctionName'
FUNCTION_NAME=back2ussr-user-get
aws lambda delete-function --function-name $FUNCTION_NAME
```

### examples
* https://d1.awsstatic.com/whitepapers/architecture/AWS-Serverless-Applications-Lens.pdf

### API Gateway, Lambda url, remote invocation url
```sh
google-chrome https://"$AWS_REGION".console.aws.amazon.com/apigateway/main/apis?region="$AWS_REGION"
# API -> Stages
```
### Lambda function editor, list of all functions
enter point for created Lambdas
```sh
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/lambda/home?region="$AWS_REGION"#/functions"
```

### invoke function from CLI, lambda execution
```sh
LAMBDA_NAME="function_name"
# example of lambda execution
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name $LAMBDA_NAME \
output.log

# example of lambda execution with payload
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name $LAMBDA_NAME \
--payload '{"key1": "value-1"}' \
output.log


# example of asynchronic lambda execution with payload
# !!! with SNS downstream execution !!! 
aws lambda invoke  \
--profile $AWS_PROFILE --region $AWS_REGION \
--function-name $LAMBDA_NAME \
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
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "dynamodb:PutItem",
                "dynamodb:GetItem",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:dynamodb:*:*:table/*",
                "arn:aws:logs:eu-central-1:8557202:log-group:/aws/lambda/function-name-1:*"
            ]
        }
    ]
}

```

### lambda logs, check logs
```sh
### lambda all logs
x-www-browser "https://"$AWS_REGION".console.aws.amazon.com/cloudwatch/home?region="$AWS_REGION"#logs:
### lambda part of logs
x-www-browser "https://"$AWS_REGION".console.aws.amazon.com/cloudwatch/home?region="$AWS_REGION"#logStream:group=/aws/lambda/"$LAMBDA_NAME";streamFilter=typeLogStreamPrefix"
```

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

#### aws sam aws tools for devops
for building serverless applications 
```bash
# https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
pip3 install aws-sam-cli
sam --version

sam cli init
sam deploy --guided
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
## DynamoDB
> store data in items, not in rows 
```sh
aws_service_abbr="dynamodb"
aws-cli-doc
aws-faq
aws-console
```

[documentation](https://github.com/awsdocs/amazon-dynamodb-developer-guide/tree/master/doc_source)
[documentation developer guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
[dynamodb local run](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
[dynamodb query language partiql - sql-like syntax](https://partiql.org/dql/overview.html)
[dynamodb via CLI](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.CLI.html)

### Keys
* Partition key ( Type: HASH )
* Sort key ( Type: Range )
* Unique key: primary/composite
### Secondary Index:
* local ( < 10Gb )
* global
### Query data:
* by partition key
* create secondary index
* data duplication with target partition key
### Scan data:
filter by conditions
### Batch operation:
* get
* write ( not update )


```sh
# list of tables: https://$AWS_REGION.console.aws.amazon.com/dynamodb/home?region=$AWS_REGION#tables:
aws dynamodb list-tables

TABLE_NAME=my_table
# create table from CLI
aws dynamodb wizard new-table

# create table from CLI
aws dynamodb create-table \
--table-name $TABLE_NAME \
--attribute-definitions \
  AttributeName=column_id,AttributeType=N \
  AttributeName=column_name,AttributeType=S \
--key-schema \
  AttributeName=column_id,KeyType=HASH \
  AttributeName=column_name,KeyType=RANGE \
--billing-mode=PAY_PER_REQUEST \
--region=$AWS_REGION

# describe table 
aws dynamodb describe-table --table-name $TABLE_NAME

# write item, write into DynamoDB
aws dynamodb put-item \
--table-name $TABLE_NAME \
--item '{"column_1":{"N":1}, "column_2":{"S":"first record"} }'
--region=$AWS_REGION
--return-consumed-capacity TOTAL

# update item 
aws dynamodb put-item \
--table-name $TABLE_NAME \
--key '{"column_1":{"N":1}, "column_2":{"S":"first record"} }' \
--update-expression "SET country_name=:new_name" \
--expression-attribute-values '{":new_name":{"S":"first"} }' \
--region=$AWS_REGION \
--return-value ALL_NEW

aws dynamodb update-item --table-name $TABLE_NAME \
--key '{"column_1":{"N":"1"}}' \
--attribute-updates '{"column_1": {"Value": {"N": "1"},"Action": "ADD"}}' \
--return-values ALL_NEW

# select records
aws dynamodb query \
  --table-name $TABLE_NAME \
  --key-condition-expression "column_1 = :id" \
  --expression-attribute-values '{":id":{"N":"1"}}' \
  --region=$AWS_REGION
  --output=table

aws dynamodb scan --table-name $TABLE_NAME \
--filter-expression "column_1 = :id" \
--expression-attribute-values '{":id":{"N":"1"}}'

# read all items
aws dynamodb scan --table-name $TABLE_NAME

# delete item
aws dynamodb delete-item --table-name $TABLE_NAME --key '{"column_1":{"N":"2"}}'

# delete table
aws dynamodb delete-table --table-name $TABLE_NAME
```


### put_item issue
```text
Type mismatch for key id expected: N actual: S"
```
key id must be Numeric
```json
{"id": 10003, "id_value":  "cherkavi_value3"}
```

---
## Route53
```sh
aws_service_abbr="route53"
aws-cli-doc
aws-faq
aws-console
```

### [resource record types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html)

Web application environment, application network connection
> health check ( light ) - answer from endpoint  
> health check ( heavy ) - answer from resources behind the application  
```mermaid
flowchart LR;
    d[dns] --> r
    r[Route 53] --> 
    lb[load 
       balancer]

    li[listener] --o lb

    tg[target
       group] ---o lb

    tg --> port[port]

    port --o ap[application]

    ap --o i[instance]

    i -.->|w| l[log]

    l --o cw[cloud 
    watch]

    ap <-.->|rw| db[(DB)]

    sm[secret 
       manager] --w-->
    cr[credentials]

    cr --o i
```
---
## SNS
```sh
aws_service_abbr="sns"
aws-cli-doc
aws-faq
aws-console


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
![cloud-watch](https://i.ibb.co/VtkfCBk/aws-2023-08-27-cloud-watch.jpg)  
### [alarms how to](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)
```sh
aws_service_abbr="cloudwatch"
aws-cli-doc
aws-faq
aws-console
```

```
 Metrics-----\
              +--->Events------>Alarm
  Logs-------/
+----------------------------------+
        dashboards
```
---
## CloudTrail
> control/log input/output API calls  
> can be activated inside VPC  
![cloud-trail](https://i.ibb.co/NWXWMtD/aws-2023-08-27-cloud-trail.jpg)  

---
## Kinesis
```sh
aws_service_abbr="kinesis"
aws-cli-doc
aws-faq
aws-console
```

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

```sh
# PRODUCER
# CLI v2
aws kinesis put-record --stream-name test --partition-key user1 --data "user signup" --cli-binary-format raw-in-base64-out
# CLI v1
aws kinesis put-record --stream-name test --partition-key user1 --data "user signup"


# CONSUMER 
# describe the stream
aws kinesis describe-stream --stream-name test
# Consume some data
aws kinesis get-shard-iterator --stream-name test --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON
aws kinesis get-records --shard-iterator <>
```

---
## KMS
```sh
aws_service_abbr="kms"
aws-cli-doc
aws-faq
aws-console
```

```sh
# 1) encryption
aws kms encrypt --key-id alias/tutorial --plaintext fileb://ExampleSecretFile.txt --output text --query CiphertextBlob  --region eu-west-2 > ExampleSecretFileEncrypted.base64
# base64 decode
cat ExampleSecretFileEncrypted.base64 | base64 --decode > ExampleSecretFileEncrypted

# 2) decryption
aws kms decrypt --ciphertext-blob fileb://ExampleSecretFileEncrypted   --output text --query Plaintext > ExampleFileDecrypted.base64  --region eu-west-2
# base64 decode
cat ExampleFileDecrypted.base64 | base64 --decode > ExampleFileDecrypted.txt

certutil -decode .\ExampleFileDecrypted.base64 .\ExampleFileDecrypted.txt
```

---
## CloudFormation
```sh
aws_service_abbr="cloudformation"
aws-cli-doc
aws-faq
aws-console

# cloudformation designer web
google-chrome "https://"$AWS_REGION".console.aws.amazon.com/cloudformation/designer/home?region="$AWS_REGION
```

```sh
aws cloudformation describe-stacks --region us-east-1
```

### CloudFormation macros
![cloud formation macros](https://i.postimg.cc/BQMY5xmx/aws-cloud-formation-macros.png)

---
## OpsWorks
> configuration management service that helps you configure and operate applications in a cloud enterprise by using Puppet or Chef. 

---
## Security  
### security links
* [Actions, resources and condition keys](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3.html)
* [AWS Security Documentation](https://docs.aws.amazon.com/security/)
* [AWS re:Invent 2022 - A day in the life of a billion requests (SEC404)](https://youtu.be/tPr1AgGkvc4)
* [Security best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)

### Sign api request
* [how to sign your AWS API request, AWS Signature Version 4 for API requests](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv.html)
* [How to Create a signed AWS API request](https://docs.aws.amazon.com/pdfs/IAM/latest/UserGuide/iam-ug.pdf#create-signed-request)
* [Authenticating Requests](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html)

### Threat model 
* [The Ultimate Beginner's Guide to Threat Modeling](https://shostack.org/resources/threat-modeling)
* [Thread model](https://catalog.workshops.aws/threatmodel/en-US/introduction)
* [open source tool for threat modeling](https://awslabs.github.io/threat-composer/workspaces/Threat%20Composer/dashboard)
* [Threat Modeling Workshop - build your ThreatModel](https://catalog.workshops.aws/threatmodel/)
* [how to integrate threat modeling into your organization’s application development lifecycle ](https://aws.amazon.com/blogs/security/how-to-approach-threat-modeling/)
* [How to approach threat modelling (AWS Summit ANZ 2021)](https://youtu.be/GuhIefIGeuA)

---
## CodeBuild
A fully managed continuous integration service ( to build, test, and package source code )
```sh
aws_service_abbr="codebuild"
```
### [codebuild docker images](https://github.com/aws/aws-codebuild-docker-images)
### codebuild start locally
#### prepare local docker image
```sh
git clone https://github.com/aws/aws-codebuild-docker-images.git
cd aws-codebuild-docker-images/ubuntu/standard/7.0/
docker build -t aws/codebuild/standard:7.0 .
```
#### download script for local run
```sh
# https://github.com/aws/aws-codebuild-docker-images/blob/master/local_builds/codebuild_build.sh
wget https://raw.githubusercontent.com/aws/aws-codebuild-docker-images/master/local_builds/codebuild_build.sh
```
#### minimal script
[build environment variables](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html)
```sh
echo 'version: 0.2
phases:
    pre_build:
        commands:
            - echo "script 01.01"
            - echo "script 01.02"
    build:
        commands:
            - echo "script 02.01"
            - echo "script 02.02"
            - echo "script 02.03"
' > buildspec-example.yml
```
#### run script 
```sh
# https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-available.html
/bin/bash codebuild_build.sh \
-i aws/codebuild/standard:7.0 \
-a /tmp/cb \
-b buildspec-example.yml \
-s `pwd` -c -m 
```

### create project
```sh
aws codebuild list-projects 

# aws codebuild batch-get-projects --names aws-dockerbuild-push2ecr
aws codebuild create-project --cli-input-json file://codebuild-project.json
```
---
## EKS Elastic Kubernetes Service
A managed Kubernetes service ( deploy, manage, and scale containerized applications using Kubernetes )
> Leader Nodes == Control Plane
```sh
aws_service_abbr="eks"

# kubectl setup 
CLUSTER_NAME=my_cluster_name
aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME

# list of clusters and nodegroups
for each_cluster in `aws eks list-clusters | jq -r .clusters[]`; do
    echo "cluster: $each_cluster"
    aws eks list-nodegroups --cluster-name $each_cluster
    echo "-------"
done
```

### EKS Cluster
*Policies:* AmazonEKSClusterPolicy  

### EKS Node (Group)
*Policies mandatory:* AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, AmazonEKS_CNI_Policy, AmazonEMRReadOnlyAccessPolicy_v2  
*Policies not mandatory:* CloudWatchAgentServerPolicy

### Install CloudWatch agent
```sh
ClusterName=$YOUR_CLUSTER_NAME_HERE
RegionName=$YOUR_AWS_REGION_HERE
FluentBitHttpPort='2020'
FluentBitReadFromHead='Off'
FluentBitReadFromTail='On'
FluentBitHttpServer='On'
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${RegionName}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f -
```

### typical amount of pods in kube-system
```sh
# kubectl get pods -n kube-system
aws-node 
coredns
kube-proxy
metrics-server
```

---
## ECS Elastic Container Service 
![ecs](https://i.ibb.co/gJwP5vm/aws-2023-08-27-ecs.jpg)  
![ecs](https://i.ibb.co/MM6jvJY/2023-08-27-2git-aws-ecs.jpg)  

ECS on Fargate
1. create docker image
2. create repository 
```sh
  aws ecr create-repository --repository-name my-name;    
  aws ecr describe-repositories --query 'repositories[].[repositoryName, repositoryUri]' --output table
  export REPOSITORY_URI=$(aws ecr describe-repositories --query 'repositories[].[repositoryUri]' --output text)
  echo ${REPOSITORY_URI}
```
3. docker login
```sh
export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
export AWS_REGION=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
```
4. tag docker image `docker tag my-image:latest ${REPOSITORY_URI}:latest`
5. push docker image: `docker push ${REPOSITORY_URI}:latest`  
   `aws ecr describe-images --repository-name my-image`
6. `aws ecs create-cluster --cluster-name my-cluster`
7. `aws ecs register-task-definition --cli-input-json file://task_definition.json`
8. `aws ecs create-service --cli-input-json file://service.json`
9. `aws ecs describe-clusters --cluster my-cluster`
   "runningTasksCount": 1
   
---
## ECR AWS Elastic Container Registry: 
A fully managed Docker container registry ( store, manage, and deploy docker images for EKS)
```sh
aws_service_abbr="ecr"
```
### [create repository](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-container-image.html)
```sh
aws_ecr_repository_name=udacity-cherkavi
aws ecr create-repository  --repository-name $aws_ecr_repository_name  --region $AWS_REGION
# aws ecr delete-repository --repository-name udacity-cherkavi

# list of all repositories 
aws ecr describe-repositories 

# list of all images in repository
aws ecr list-images  --repository-name $aws_ecr_repository_name 
```

### docker login
```sh
# login with account id
AWS_ACCOUNT_ID=`aws sts get-caller-identity --query Account | jq -r . `
docker login -u AWS -p $(aws ecr get-login-password --region ${AWS_REGION}) ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

# login with aws username/password
aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

# check connection
aws ecr get-authorization-token
```

### docker push local container to ECR
```sh
aws_ecr_repository_uri=`aws ecr describe-repositories --repository-names $aws_ecr_repository_name | jq -r '.repositories[0].repositoryUri'`
echo $aws_ecr_repository_uri

docker_image_local=050db1833a9c
docker_image_remote_tag=20230810
docker_image_remote_name=$aws_ecr_repository_uri:$docker_image_remote_tag
docker tag  $docker_image_local $docker_image_remote_name

# push to registry
docker push $docker_image_remote_name

# pull from 
docker pull $docker_image_remote_name
```

---
## STS Security Token Service 
```sh
aws_service_abbr="sts"

# get account get user id
aws sts get-caller-identity 
aws sts get-caller-identity --query Account
aws sts get-caller-identity --query UserId
# aws sts get-access-key-info --access-key-id 
```

---
## [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)
> logic orchestrator
> AWS Glue can be one of the step ( similar service )
* [workflow studio for building step functions](https://docs.aws.amazon.com/step-functions/latest/dg/workflow-studio.html)
* [state between calls - execution history](https://docs.aws.amazon.com/step-functions/latest/apireference/API_GetExecutionHistory.html)

---
## Code Commit
### how to clone remote repository
1. create new profile (aws_profile_in_aws_credentials) in ~/.aws/credentials
2. execute next commands
```sh
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
AWS_PROFILE=aws_profile_in_aws_credentials
git config --global credential.helper "!aws --profile $AWS_PROFILE codecommit credential-helper $@"
```
3. check your settings:
```sh
cat ~/.gitconfig
# check the section: 
# [credential]
#         helper = "!aws --profile aws_profile_in_aws_credentials codecommit credential-helper $@"
#         UseHttpPath = true
# !!! should be not like:
# helper = "aws s3 ls --profile aws_profile_in_aws_credentials codecommit credential-helper "

# fix it otherwise
git config --global --edit 
```
4. clone your repo with https

## Code Build
```mermaid
flowchart RL;
    subgraph buildtime
        pr[project] --o cb[Code Build]
        r[role] --o pr
        pl[policy] --o r
        s3[s3] --o pr

        git[git 
            repo] -.->|w| pr

        pr -.-> 
        i[container 
        image]

        i --o ecr[ECR]

        i -.-> d[deployment]
    end

    subgraph runtime
        p[pod] --o n[node]

        n --o 
        ng[node 
           group]

        ng --o eks
        c[Cluster] --o eks[EKS]
    end

    d ----|service| p

```
[orchestration tool](https://pypi.org/project/aws-codeseeder/)
[orchestration tool documentation]( https://seed-farmer.readthedocs.io/en/latest)
```sh
pip install seed-farmer
```
in case of issue: 
```
AttributeError: module 'lib' has no attribute 'X509_V_FLAG_CB_ISSUER_CHECK'
```
```sh
pip3 install pyOpenSSL --upgrade
```

---
## [AWS Batch]()

---
## Cognito
* [Amazon Cognito](https://youtu.be/1vYUt2u2EB0)
* [How to create an Amazon Cognito user pool](https://youtu.be/SZqZ3dXO7Jo)
* [Getting Started with Amazon Cognito](https://youtu.be/OAR4ZHP8DEg)

> authentication, authorization, user pool, identity pool

---
## [Fargate](https://aws.amazon.com/fargate/)
> run containers as Lambda functions

---
## Shield
![schield](https://i.ibb.co/n11WkM1/aws-2023-08-27-aws-schield-png.jpg)  

---
## Firewall
![firewall](https://i.ibb.co/Qn4WC9z/aws-2023-08-27-aws-web-application-firewall-png.jpg)  


---
## Launch Template
```mermaid
flowchart LR;
    lt[Launch 
       Template] --o 
    as[Auto
    Scaling]
```

---
## Launch Configuration
```mermaid
flowchart LR;
    lt[Launch 
       Configuration] --o 
    as[Auto
    Scaling]
```
---
## Glue
> Extract Transform Load 
> see: Data Pipeline
```mermaid
flowchart LR;

s[source] -.-> gc[Glue Crawler] -.-> gdc[Glue  Data Catalog] -.-> etl[ETL jobs] -.-> d[dest]

rds[RDS] --extend--> s
S3 --extend--> d
rds ~~~ d
```

---
## Amazon X-Ray
http header for tracing: x-amzn-trace-id

---
## cloud9

---
## Code Services
* source
  * code repository
  * vscode: aws toolkits, amazon q
* build
  * codePipeline
    * codeguru
    * codebuild
    * codeartifacts
* deploy
  * codedeploy
* monitor
  * X-Ray
---
## QuickSight

---
## cloudsearch

---
## ElasticSearch

---
## personal health dashboard

---
## app sync

---
## FIS - Fault Injection Service
> stress an application by creating disruptive events so that you can observe how your application responds ( chaos engineering ).  

---
## examples
![image](https://github.com/cherkavi/cheat-sheet/assets/8113355/658be5b5-2898-413e-ae46-d5fb81ac1add)  

---
## upcoming courses todo
* https://www.examtopics.com/exams/amazon/aws-certified-cloud-practitioner
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
* https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html
* [redshift](https://aws.amazon.com/redshift/)
