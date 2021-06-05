[documentation](https://mariadb.com/kb/en/)

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


### import db export db, archive, backup db, restore
```sh
# prerequisites
## try just to connect to db
mysql --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --database=masterdb --password=my_passw
## docker mysql client 
docker run -it mariadb mysql --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --database=masterdb --password=my_passw
## enable server logging !!!
### server-id = 1
### log_bin = /var/log/mysql/mysql-bin.log
sudo sed -i '/server-id/s/^#//g' /etc/mysql/mysql.conf.d/mysqld.cnf && sudo sed -i '/log_bin/s/^#//g' /etc/mysql/mysql.conf.d/mysqld.cnf

# mysqldump installation
# sudo apt install mysql-client-core-8.0

### backup ( pay attention to 'database' key )
mysqldump --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw masterdb > backup.sql
# 'Access denied; you need (at least one of) the PROCESS privilege(s) for this operation' when trying to dump tablespaces
mysqldump --no-tablespaces --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw masterdb > backup.sql
mysqldump --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw --databases masterdb > backup.sql
mysqldump --extended-insert --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw --databases masterdb | sed 's$),($),\n($g' > backup.sql
mysqldump --extended-insert=FALSE --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw --databases masterdb > backup.sql
mysqldump --databases ghost_prod --master-data=2 --single-transaction --order-by-primary -r backup.sql -u ghost -p

# backup only selected tables 
mysqldump --extended-insert=FALSE  --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw masterdb table_1 table_2 > backup.sql

# backup only selected table with condition 
mysqldump --extended-insert=FALSE  --host=mysql-dev-eu.a.db.ondigitalocean.com --user=admin --port=3060 --password=my_passw masterdb table_1 --where="column_1=0" --no-create-info  > backup.sql


### restore #1
mysql -u mysql_user -p DATABASE < backup.sql
# restore #2 
mysql -u mysql_user -p 
use DATABASE
source /home/user/backup.sql

```

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
```
source /path/to/file.sql
```
* shell command
```
mysql -h hostname -u user database < path/to/test.sql
```
* via docker
```
docker run -it --volume $(pwd):/work mariadb mysql --host=eu-do-user.ondigitalocean.com --user=admin --port=25060 --database=master_db --password=pass
# docker exec --volume $(pwd):/work -it mariadb
source /work/zip-code-us.sql
```

### show databases and switch to one of them:
```
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


### version
SELECT VERSION();

### example of spring config
* MariaDB
```
ds.setMaximumPoolSize(20);
ds.setDriverClassName("org.mariadb.jdbc.Driver");
ds.setJdbcUrl("jdbc:mariadb://localhost:3306/db");
ds.addDataSourceProperty("user", "root");
ds.addDataSourceProperty("password", "myPassword");
ds.setAutoCommit(false);
jdbc.dialect:
  org.hibernate.dialect.MariaDBDialect
  org.hibernate.dialect.MariaDB53Dialect
```

* MySQL
```
jdbc.driver: com.mysql.jdbc.Driver
jdbc.dialect: org.hibernate.dialect.MySQL57InnoDBDialect
jdbc:mysql://localhost:3306/bpmnui?serverTimezone=Europe/Brussels
```

### maven dependency
* MySQL
```
ds.setDriverClassName("com.mysql.jdbc.Driver");
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>6.0.6</version>
</dependency>
```

* MariaDB
```
ds.setDriverClassName("org.mariadb.jdbc.Driver");
<dependency>
    <groupId>org.mariadb.jdbc</groupId>
    <artifactId>mariadb-java-client</artifactId>
    <version>2.2.4</version>
</dependency>
```

### create database:
```
DROP DATABASE IF EXISTS {databasename};
CREATE DATABASE {databasename}
  CHARACTER SET = 'utf8'
  COLLATE = 'utf8_general_ci';
```

### create table, autoincrement
```
create table IF NOT EXISTS `hlm_auth_ext`(
  `auth_ext_id` bigint NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `uuid` varchar(64) NOT NULL,
)  ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
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


### subquery returns more than one row
```
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
```
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

# issues
## insert datetime issue
```
-- `date_start_orig` datetime NOT NULL,
(1292, "Incorrect datetime value: '0000-00-00 00:00:00' for column 'date_start_orig' at row 1")
```
to cure it:
```
'1970-01-02 00:00:00'
-- or 
SET SQL_MODE='ALLOW_INVALID_DATES';
```
