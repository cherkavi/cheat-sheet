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

### show databases and switch to one of them:
```
show databases;
use {databasename};
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
```

* MySQL
```
jdbc.driver: com.mysql.jdbc.Driver
jdbc.dialect:
	MariaDBDialect
	MariaDB53Dialect
	org.hibernate.dialect.MySQL57InnoDBDialect
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

###execute sql file 
'db.dialect' : 'org.hibernate.dialect.MySQL57InnoDBDialect',
'jdbc.driver': 'com.mysql.jdbc.Driver',
'jdbc.user' : 'hibernate_orm_test',
'jdbc.pass' : 'hibernate_orm_test',
'jdbc.url' : 'jdbc:mysql://127.0.0.1/hibernate_orm_test'


create during startup:
MYSQL_DATABASE=activitidb
