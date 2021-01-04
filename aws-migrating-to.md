## [database migration](https://aws.amazon.com/blogs/database/database-migration-what-do-you-need-to-know-before-you-start/)

## [Migration strategies](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
* rehost
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
* replatform
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
* repurchase
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
* refactor reachitect
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
* retire
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
* retain
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
