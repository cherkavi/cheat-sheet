# MariaDB
### links 
* [documentation](https://mariadb.com/kb/en/)
* [docker-compose: mariadb with GUI client](https://github.com/cherkavi/docker-images/blob/master/mariadb-mysql/README.md)

### execute docker container ( utf8 ):
```
docker pull mariadb

docker run --name mysql-container --volume /my/local/folder/data:/var/lib/mysql --publish 3306:3306 --env MYSQL_ROOT_PASSWORD=root --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### execute docker container, create DB with specific name, execute all scripts in folder:
```
docker pull mariadb

docker run --name mysql-container --volume /my/local/folder/data:/var/lib/mysql --volume /my/path/to/sql:/docker-entrypoint-initdb.d --publish 3306:3306 --env MYSQL_ROOT_PASSWORD=root --env MYSQL_DATABASE={databasename} --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

### config file, conf file
```sh
cat /etc/mysql/mysql.conf.d/mysqld.cnf
```

### connect to mysql using shell tool:
```
mysql --user=root --password=root
mysql --user=root --password=root --host=127.0.0.1 --port=3306
```
```
docker exec -it mysql-container  /usr/bin/mysql  --user=root --password=root
```

### mycli connect
```bash
mycli mysql://user_name:passw@mysql_host:mysql_port/schema
# via ssh 
mycli --ssh-user heritageweb --ssh-host 55.55.55.54 mysql://user_name:passw@mysql_host:mysql_port/schema
#   --ssh-user, --ssh-password
```
#### .myclirc settings for vertical output
```properties
auto_vertical_output = True
```

#### mycli execute script, mycli 
```
mycli mysql://user_name:passw@mysql_host:mysql_port/schema --execute "select * from dual"
mycli mysql://user_name:passw@mysql_host:mysql_port/schema < some-sql.txt
```
```
mycli mysql://user_name:passw@mysql_host:mysql_port/schema
help;
source some-sql.txt;
```


### import db export db, archive, backup db, restore db, recovery db
* [backup recovery](https://dev.mysql.com/doc/refman/8.0/en/backup-and-recovery.html)
* [backup policies](https://dev.mysql.com/doc/refman/8.0/en/backup-policy.html):
* [dump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)
* [replication](https://dev.mysql.com/doc/refman/8.0/en/replication.html)
  * [replication solutions](https://dev.mysql.com/doc/refman/8.0/en/replication-solutions.html)
  * [replication implementation](https://dev.mysql.com/doc/refman/8.0/en/replication-implementation.html)
  * [replication notes and tips](https://dev.mysql.com/doc/refman/8.0/en/replication-notes.html)
  * [replication FAQ](https://dev.mysql.com/doc/refman/8.0/en/faqs-replication.html)
  * [replicating to slaves](https://dev.mysql.com/doc/refman/8.0/en/replication-solutions-partitioning.html)
#### prerequisites
```sh
## try just to connect to db
mysql --host=mysql-dev-eu.a.db.ondigitalocean.com --port=3060 --user=admin --password=my_passw --database=masterdb 
## docker mysql client 
docker run -it mariadb mysql --host=mysql-dev-eu.a.db.ondigitalocean.com --port=3060 --user=admin --database=masterdb --password=my_passw
## enable server logging !!!
### server-id = 1
### log_bin = /var/log/mysql/mysql-bin.log
sudo sed -i '/server-id/s/^#//g' /etc/mysql/mysql.conf.d/mysqld.cnf && sudo sed -i '/log_bin/s/^#//g' /etc/mysql/mysql.conf.d/mysqld.cnf
```
#### mysql dump mariadb dump
```sh
# mysqldump installation
# sudo apt install mysql-client-core-8.0
```
```sh
MY_HOST="mysql-dev-eu.a.db.ondigitalocean.com"
MY_USER="admin"
MY_PASS="my_pass"
MY_PORT="3060"
MY_DB="masterdb"
MY_BACKUP="backup.sql"
### backup ( pay attention to 'database' key )
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS $MY_DB > $MY_BACKUP
# 'Access denied; you need (at least one of) the PROCESS privilege(s) for this operation' when trying to dump tablespaces
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS --no-tablespaces  $MY_DB > $MY_BACKUP
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS --databases $MY_DB > $MY_BACKUP
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS --databases $MY_DB --extended-insert | sed 's$),($),\n($g' > $MY_BACKUP
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS --databases $MY_DB --extended-insert=FALSE  > $MY_BACKUP
mysqldump --databases ghost_prod --master-data=2 -u $MY_USER -p --single-transaction --order-by-primary -r $MY_BACKUP

# backup only selected tables 
mysqldump --host=$MY_HOST --port=$MY_PORT --user=$MY_USER --password=$MY_PASS --extended-insert=FALSE $MY_DB table_1 table_2 > $MY_BACKUP
# backup only selected table with condition 
mysqldump --host=$MY_HOST --user=$MY_USER --port=$MY_PORT --password=$MY_PASS --extended-insert=FALSE --where="column_1=0" --no-create-info $MY_DB table_1 > $MY_BACKUP
```
```sh
### restore #1
mysql -u $MY_USER -p $MY_DB < $MY_BACKUP
# restore #2 
mysql -u $MY_USER -p 
use DATABASE
source /home/user/backup.sql
```

#### dump
* manual start and stop 
  * make the server ReadOnly
  ```bash
  mysql> FLUSH TABLES WITH READ LOCK;
  mysql> SET GLOBAL read_only = ON;
  ```
  * make dump
  * back server to Normal Mode
  ```bash
  mysql> SET GLOBAL read_only = OFF;
  mysql> UNLOCK TABLES;    
  ```
* with one command without manual step
  * mysqldump --master-data --single-transaction > backup.sql

#### dump tools
* [ubuntu utility: automysqlbackup](https://www.digitalocean.com/community/tutorials/how-to-backup-mysql-databases-on-an-ubuntu-vps)
* raw solution via cron
  ```sh
  crontab -e
  # And add the following config
  50 23 */2 * * mysqldump -u mysqldump DATABASE | gzip > dump.sql.gz >/dev/null 2>&1
  ```        
* [raw to s3cmd (amazon, digitalocean...) ](https://www.danielord.co.uk/backup-mysql-database-digital-ocean-spaces)
* dump with binary log position
  ```bash
  # innodb
  mysqldump --single-transaction --flush-logs --master-data=2 --all-databases --delete-master-logs > backup.sql
  # not innodb
  mysqldump --lock-tables
  ```

#### cold backup (copy raw files)
If you can shut down the MySQL server, 
you can make a physical backup that consists of all files used by InnoDB to manage its tables. 
Use the following procedure:
* Perform a slow shutdown of the MySQL server and make sure that it stops without errors.
* Copy all InnoDB data files (ibdata files and .ibd files)
* Copy all InnoDB log files (ib_logfile files)
* Copy your my.cnf configuration file

#### backup issue: during backup strange message appears: "Enter password:" even with password in command line
```sh
vim .my.cnf
```
```properties
[mysqldump]
user=mysqluser
password=secret
```
```sh
# !!! without password !!!
mysqldump --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 masterdb table_1 table_2 > backup.sql
```

### execute sql file with mysqltool
* inside mysql 
  ```sh
  source /path/to/file.sql
  ```
* shell command
  ```sh
  mysql -h hostname -u user database < path/to/test.sql
  ```
* via docker
  ```sh
  docker run -it --volume $(pwd):/work mariadb mysql --host=eu-do-user.ondigitalocean.com --user=admin --port=25060 --database=master_db --password=pass
  # docker exec --volume $(pwd):/work -it mariadb
  source /work/zip-code-us.sql
  ```

### show databases and switch to one of them:
```sql
show databases;
use {databasename};
```

### user <-> role
![user role relationship](https://i.postimg.cc/bv0dRDrg/mysql-user-role.png)

#### users 
```sql
show grants;
SELECT host, user FROM mysql.user where user='www_admin'
```

#### grant access
```sql
GRANT ALL PRIVILEGES ON `www\_masterdb`.* TO `www_admin`@`%`;
```

### print all tables
```sql
show tables;
```

### print all tables and all columns
```sql
select table_name, column_name, data_type from information_schema.columns
 where TABLE_NAME like 'some_prefix%'
order by TABLE_NAME, ORDINAL_POSITION
```

### print all columns in table, show table structure
```sql
describe table_name;
show columns from table_name;
select * from information_schema.columns where TABLE_NAME='listings_dir' and COLUMN_NAME like '%PRODUCT%';
```
```sh
mycli mysql://user:passw@host:port/schema --execute "select table_name, column_name, data_type  from information_schema.columns where TABLE_NAME like 'hlm%' order by TABLE_NAME, ORDINAL_POSITION;" | awk -F '\t' '{print $1","$2","$3}' > columns
```

### add column 
```sql
-- pay attention to quotas around names
ALTER TABLE `some_table` ADD `json_source` varchar(32) NOT NULL DEFAULT '';
-- don't use 'ALTER COLUMN'
ALTER TABLE `some_table` MODIFY `json_source` varchar(32) NULL;
```
### rename column
```
alter table messages rename column sent_time to sent_email_time;
```

### mysql version
SELECT VERSION();

### example of spring config
* MariaDB
  ```java
  ds.setMaximumPoolSize(20);
  ds.setDriverClassName("org.mariadb.jdbc.Driver");
  ds.setJdbcUrl("jdbc:mariadb://localhost:3306/db");
  ds.addDataSourceProperty("user", "root");
  ds.addDataSourceProperty("password", "myPassword");
  ds.setAutoCommit(false);
  // jdbc.dialect:
  //   org.hibernate.dialect.MariaDBDialect
  //   org.hibernate.dialect.MariaDB53Dialect
  ```
* MySQL
  ```
  jdbc.driver: com.mysql.jdbc.Driver
  jdbc.dialect: org.hibernate.dialect.MySQL57InnoDBDialect
  jdbc:mysql://localhost:3306/bpmnui?serverTimezone=Europe/Brussels
  ```

### maven dependency
* MySQL
  ```java
  ds.setDriverClassName("com.mysql.jdbc.Driver");
  ```
  ```xml
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>6.0.6</version>
  </dependency>
  ```
* MariaDB
  ```java
  ds.setDriverClassName("org.mariadb.jdbc.Driver");
  ```
  ```xml
  <dependency>
      <groupId>org.mariadb.jdbc</groupId>
      <artifactId>mariadb-java-client</artifactId>
      <version>2.2.4</version>
  </dependency>
  ```

### create database:
```sql
DROP DATABASE IF EXISTS {databasename};
CREATE DATABASE {databasename}
  CHARACTER SET = 'utf8'
  COLLATE = 'utf8_general_ci'; 
-- 'utf8_general_ci' - case insensitive
-- 'utf8_general_cs' - case sensitive
```

### create table, autoincrement
```sql
create table IF NOT EXISTS `hlm_auth_ext`(
  `auth_ext_id` bigint NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `uuid` varchar(64) NOT NULL,
)  ENGINE=InnoDB AUTO_INCREMENT=10001 DEFAULT CHARSET=utf8;
```

### create column if not exists
```
ALTER TABLE test ADD COLUMN IF NOT EXISTS column_a VARCHAR(255);
```

### show ddl, ddl for table, ddl table
```
SHOW CREATE TABLE yourTableName;
```

### table indexes of table
```sql
SHOW INDEX FROM my_table_name;
```

### table constraints of table, show constraints
```sql
select COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_COLUMN_NAME, REFERENCED_TABLE_NAME from information_schema.KEY_COLUMN_USAGE where TABLE_NAME = 'my_table_name';
```

### subquery returns more than one row, collect comma delimiter, join columns in one group columns
```sql
select 
    u.name_f, 
    u.name_l, 
    (select GROUP_CONCAT(pp.title, '')
    from hlm_practices pp where user_id=100
    )
from hlm_user u 
where u.user_id = 100;
```

### date diff, compare date, datetime substraction
```sql
----- return another date with shifting by interval
-- (now() - interval 1 day)
----- return amount of days between two dates
-- datediff(now(), datetime_field_in_db )
```

### value substitution string replace
```sql
select (case when unsubscribed>0 then 'true' else 'false' end) from lm_user limit 5;
```

# check data
```sql
-- check url
SELECT url FROM licenses WHERE length(url)>0 and url NOT REGEXP '^http*';
-- check encoding
SELECT subject FROM mails WHERE subject <> CONVERT(subject USING ASCII);
-- check email address
SELECT email FROM mails WHERE length(email)>0 and upper(email) NOT REGEXP '^[a-zA-Z0-9+_.-]+@[a-zA-Z0-9.-]+$';
```

# Search

## simple search
### case search, case sensitive, case insensitive
```sql
-- case insensitive search
AND q.summary LIKE '%my_criteria%'

-- case sensitive ( if at least one operand is binary string )
AND q.summary LIKE BINARY '%my_criteria%'
```

## similar search
### sounds like
```sql
select count(*) from users where soundex(name_first) = soundex('vitali');
select count(*) from listing where soundex(description) like concat('%', soundex('asylum'),'%');
```

## full text fuzzy search
### search match 
```sql
SELECT pages.*,
       MATCH (head, body) AGAINST ('some words') AS relevance,
       MATCH (head) AGAINST ('some words') AS title_relevance
FROM pages
WHERE MATCH (head, body) AGAINST ('some words')
ORDER BY title_relevance DESC, relevance DESC
```

# custom function, UDF
```sql
DROP FUNCTION if exists `digits_only`;
DELIMITER $$

CREATE FUNCTION `digits_only`(in_string VARCHAR(100) CHARACTER SET utf8)
RETURNS VARCHAR(100)
NO SQL
BEGIN

    DECLARE ctrNumber VARCHAR(50);
    DECLARE finNumber VARCHAR(50) DEFAULT '';
    DECLARE sChar VARCHAR(1);
    DECLARE inti INTEGER DEFAULT 1;

    -- swallow all exceptions, continue in any exception
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
    BEGIN

    END;

    IF LENGTH(in_string) > 0 THEN
        WHILE(inti <= LENGTH(in_string)) DO
            SET sChar = SUBSTRING(in_string, inti, 1);
            SET ctrNumber = FIND_IN_SET(sChar, '0,1,2,3,4,5,6,7,8,9');
            IF ctrNumber > 0 THEN
                SET finNumber = CONCAT(finNumber, sChar);
            END IF;
            SET inti = inti + 1;
        END WHILE;
        IF LENGTH(finNumber) > 0 THEN
            RETURN finNumber;
        ELSE
            RETURN NULL;
        END IF;
    ELSE
        RETURN NULL;
    END IF;
END$$
DELIMITER ;

```

## issues
### insert datetime issue
```sql
-- `date_start_orig` datetime NOT NULL,
(1292, "Incorrect datetime value: '0000-00-00 00:00:00' for column 'date_start_orig' at row 1")
```
to cure it:
```sql
'1970-01-02 00:00:00'
-- or 
SET SQL_MODE='ALLOW_INVALID_DATES';
```
