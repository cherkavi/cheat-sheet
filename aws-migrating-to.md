## [database migration](https://aws.amazon.com/blogs/database/database-migration-what-do-you-need-to-know-before-you-start/)

[TSO, Migration evaluation](https://aws.amazon.com/migration-evaluator/)
[System Manager, System Manager Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)

## [Migration strategies](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
> evaluate->make plans->test plans->reformat plan->test plan->performing
* rehost ( one-to-one )
  * Specify Migration Goals
  * Identify data to migrate
  * Discover components to migrate
  * Analyze migration services
  * Identify migration plan
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test
* replatform (one-to-one with optimization, replace with existing AWS services )
  * Specify Migration Goals
  * Identify data to migrate
  * Discover components to migrate
  * **Evaluate appropriate service replacements**
  * Analyze migration services
  * Identify migration plan
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test
* repurchase ( using AWS services + AWS Marketplace with configured machines )
  * Specify Migration Goals
  * Identify data to migrate
  * Discover components to migrate
  * **Evaluate appropriate service replacements**
  * Analyze migration services
  * Identify migration plan
  * **Purchase necessary software/components**
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test
* refactor reachitect ( what should be re-architected -> SOA )
  * Specify Migration Goals
  * Identify data to migrate
  * Discover components to migrate
  * **Evaluate appropriate service replacements**
  * **Identify appropriate architecture**
  * Analyze migration services
  * Identify migration plan
  * **Build test architecture**
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test
* retire ( what may not be migrated )
  * Specify Migration Goals
  * Identify data to migrate
  * **Identify unnecessary components**
  * **Discover components to migrate**
  * **Retire unnecessary components**
  * Analyze migration services
  * Identify migration plan
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test
* retain ( not ready, not necessary to migrate )
  * Specify Migration Goals
  * Identify data to migrate
  * **Identify best environment for each component**
  * Analyze migration services
  * Identify migration plan
  * Setup cross-environment connectivity
  * Test components and application functionality
  * Backup data and application
  * Replicate application data
  * Migrate application components
  * Test

[How to migrate](https://aws.amazon.com/cloud-migration/how-to-migrate/)
## DB migration
### Requirements
* Platform review and considerations
* Key parameters of new environment
* Can app affort downtime, how long ?
* Data migration - now or later ?
### Network limitation

^|------------------------------|------------|
T| DirectConnect/StorageGateway | Snowmobile |
i|------------------------------|------------|
m| transfer to S3 directly      | Snowball   |
e|------------------------------|------------|
                  size of data ---> 


## AWS Server Migration Service
### requirements
* available for: VMware vSphere, Microsoft Hyper-V, Azure VM
* replicate to AmazonMachineImages ( EC2 )
* using connector - BSDVM that you should install into your environment

## AWS Migration Hub
* Amazon CloudEndure, 
* AWS ServerMigrationService
* AWS DatabaseMigrationService
* Application Discovery Service 
* Application Discovery Agents

## Application Discovery Service
> perform discovery and collect data
* agentless ( working with VMware vCenter )
* agent-based ( collecting processes into VM and exists network connections )

## Migration steps
* Disover current infrastructure
* Experiment with services and copy of data
* Iterate with another experiment ( using other services )
* Deploying to AWS

## Percona XtraBackup
### installation
```sh
wget https://repo.percona.com/apt/percona-release_latest.$(lsb_release -sc)_all.deb
sudo dpkg -i percona-release_latest.$(lsb_release -sc)_all.deb
sudo apt-get update
# MySQL 5.6, 5.7; for MySQL 8.0 - XtraBackup 8.0
sudo apt-get -y install percona-xtrabackup-24
```

### create backup
```
sudo xtrabackup --backup --stream=tar --user=my_user --password=my_password | gzip -c > /home/ubuntu/xtrabackup.tar.gz
```

### list of policies for role
```
AWSApplicationDiscoveryAgentAccess
AmazonS3FullAccess
AWSCloud9Administrator
RDS-Policy
CF-Policy
IAM-Policy
```

### create new instance with db
```
aws rds restore-db-instance-from-s3  --db-instance-identifier ghost-db-mysql --db-instance-class db.t2.large  --engine mysql  --source-engine mysql  --source-engine-version 5.7.22  --s3-bucket-name <FMI>  --s3-ingestion-role-arn "<FMI>"  --allocated-storage 100 --master-username admin  --master-user-password foobarfoobar --region <FMI>
```

### check status
```
aws rds describe-db-instances --db-instance-identifier ghost-db-mysql --region <FMI> --query DBInstances[0].DBInstanceStatus
```

## Database migration service
* create replication instance
* create source endpoint
* create target endpoint
* create database migration task

## [CloudEndure](https://aws.amazon.com/cloudendure-migration/)
* [registration](www.cloudendure.com), get free migration license
* [IAM policy](https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-Migration/lab-4-cloud-endure/iampolicy.json)
* Create user IAM user
* CloudEndure->select source and destination regions
* CloudEndure->Setup & Info->write down AccessKeyId & SecretAccessKey
* CloudEndure->Machines How to Add Machines
* Install agent on EC2 instance
  ```sh
  wget -O ./installer_linux.py https://console.cloudendure.com/installer_linux.py
  sudo python ./installer_linux.py -t 0B7F-A786-6F8F-0727-08E2-0CB1-B758-7E41-4D03-7A47-8C2A-129A-8CFE-B115-09EA-A1EC --no-prompt
  ```
* CloudEndure->LaunchTargetMachines->TestMode ( check JobProgress )
* EC2 instance list
* check ip addresses in new EC2