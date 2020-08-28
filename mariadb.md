[documentation](https://mariadb.com/kb/en/)

###execute docker container ( utf8 ):
```
docker pull mariadb

docker run --name mysql-container --volume /my/local/folder/data:/var/lib/mysql --publish 3306:3306 --env MYSQL_ROOT_PASSWORD=root --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

###execute docker container, create DB with specific name, execute all scripts in folder:
```
docker pull mariadb

docker run --name mysql-container --volume /my/local/folder/data:/var/lib/mysql --volume /my/path/to/sql:/docker-entrypoint-initdb.d --publish 3306:3306 --env MYSQL_ROOT_PASSWORD=root --env MYSQL_DATABASE={databasename} --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

```

###connect to mysql shell tool:
```
mysql --user=root --password=root
```
```
docker exec -it mysql-container  /usr/bin/mysql  --user=root --password=root
```

### import db export db, archive, backup, restore
```sh
# backup
mysqldump -u mysql_user -p DATABASE_NAME > backup.sql
# restore
 mysql -u mysql_user -p DATABASE < backup.sql
```

###execute sql file with mysqltool
* inside mysql 
```
source /path/to/file.sql
```
* shell command
```
mysql -h hostname -u user database < path/to/test.sql
```

### show databases and switch to one of them:
```
show databases;
use {databasename};
```

### user <-> role
![user role relationship](https://i.postimg.cc/bv0dRDrg/mysql-user-role.png)

### print all tables
```sql
show tables;
```

### print all columns in table, show table structure
```sql
describe table_name;
show columns from table_name;
select * from information_schema.columns where TABLE_NAME='listings_dir' and COLUMN_NAME like '%PRODUCT%';
```

### add column 
```sql
-- pay attention to quotas around names
ALTER TABLE `some_table` ADD `json_source` varchar(32) NOT NULL DEFAULT '';
-- don't use 'ALTER COLUMN'
ALTER TABLE `some_table` MODIFY `json_source` varchar(32) NULL;
```

### print all tables and all columns
```sql
select table_name, column_name, data_type from information_schema.columns
 where TABLE_NAME like '%some_prefix'
order by TABLE_NAME, ORDINAL_POSITION
```

###version
SELECT VERSION();

###example of spring config
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

###maven dependency
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

###create database:
```
DROP DATABASE IF EXISTS {databasename};
CREATE DATABASE {databasename}
  CHARACTER SET = 'utf8'
  COLLATE = 'utf8_general_ci';
```
