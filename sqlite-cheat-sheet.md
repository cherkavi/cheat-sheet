# SQLite cheat sheet
* [tutorial](https://alphacodingskills.com/sqlite/sqlite-tutorial.php)
* [sqlite browser](https://github.com/sqlitebrowser/sqlitebrowser#ubuntu-and-derivatives)

## Alternatives
### [h2](https://h2database.com/html/download.html)
```sh
sudo apt install h2database
```
### [hsqldb](http://hsqldb.org/)
```sh
sudo apt install hsqldb-utils
```

## sqlite install 
```sh
sudo apt-get install sqlite3 sqlite3-doc sqlite3-tool
```

## tools
* [sqldiff](https://www.sqlite.org/sqldiff.html)
* [sqlite3_analyzer](https://www.sqlite.org/sqlanalyze.html)
* [mysql db to sqlite](https://pypi.org/project/mysql-to-sqlite3/)

## [mysql to sqlite](https://github.com/cherkavi/docker-images/blob/master/mariadb-mysql/README.md#convert-mysql-to-sqlite)

## init db
```sh
sqlite3 -init db.sqlite
```

## init db and import 
```sh
# [pip3 install termsql](https://github.com/tobimensch/termsql)
termsql -i mycsvfile.CSV -d ',' -c 'field_name,field_index' -t 'index_de' -o mynewdatabase.db
```

## open db
```sh
sqlite3 mynewdatabase.db
.tables
```
or
```sh
sqlite3
.open mydatabase.db
```
or 
```sh
sqlite3
attach "mydatabase.db" as mydb;
```
## redirect output to file redirect output to stdout
```
.output result-output.txt
select * from index_de;
.output
```
## execute inline query
```sh
sqlite3 $DB_FILE "select count(*) from r_d_dxc_developer;"
```

## [create table and import](https://sqlite.org/cli.html#importing_files_as_csv_or_other_formats)
```sql
CREATE TABLE index_de(field_name TEXT NOT NULL, field_type TEXT NOT NULL );
CREATE TABLE index_cn(field_name TEXT NOT NULL, field_type TEXT NOT NULL );
-- show tables 
.tables
.schema index_cn

-- import csv to table 
.mode csv
.separator ","
.import autolabel-staging-merge.elk-index.cn.fields.csv index_cn
.import autolabel-merge.elk-index.de-prod.fields.csv index_de
```

## [sqlite export to file](https://sqlite.org/cli.html#export_to_csv)
```sql
.output autolabel-merge.fields
.shell ls -la autolabel-merge.fields
.shell cat autolabel-merge.fields

select index_cn.field_name, index_de.field_type "type_de", index_cn.field_type "type_cn"
from index_cn index_cn
inner join index_de on index_de.field_name=index_cn.field_name 
where index_cn.field_type != index_de.field_type
;
.output stdout
.exit
```

## sqlite import, read from file, write to db
### read data from command line
```sh
cat src/scripts.sql | sqlite3 src/db.sqlite
```
### read data from sql
```sql
INSERT INTO authors (id, name) VALUES (1, 'Author One');
INSERT INTO authors (id, name) VALUES (2, 'Author Two');
INSERT INTO authors (id, name) VALUES (3, 'Author Three');

INSERT INTO books (id, title, author_id) VALUES (1, 'Book One', 1);
INSERT INTO books (id, title, author_id) VALUES (2, 'Book Two', 1);
INSERT INTO books (id, title, author_id) VALUES (3, 'Book Three', 1);
INSERT INTO books (id, title, author_id) VALUES (4, 'Book Four', 2);
INSERT INTO books (id, title, author_id) VALUES (5, 'Book Four', 99);
```
```sh
sqlite3 test_database.db
```
```sql
.read src/create_tables.sql
.read src/insert_data.sql
```
```sql
SELECT a.id, a.name, b.id, b.title FROM authors a inner JOIN books b ON a.id = b.author_id;
SELECT a.id, a.name, b.id, b.title FROM authors a left  JOIN books b ON a.id = b.author_id;
SELECT a.id, a.name, b.id, b.title FROM authors a full  JOIN books b ON a.id = b.author_id;
```

## constraint example
```sql
CREATE TABLE cv_generated (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  tags TEXT,
  board INTEGER,
  position_note TEXT,
  creation_date DATETIME DEFAULT CURRENT_TIMESTAMP,

  FOREIGN KEY (board) REFERENCES board (id)
);


```
