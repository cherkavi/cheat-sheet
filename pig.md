## Pig ( PigLatin )
* all operators will be translated to MapReduce  
* accessible operations:
  * join dataset
  * sort dataset
  * filter dataset
  * data types
  * group by
  * user defined functions
* free data structure ( Hive - fixed data structure )  
* function names, names of relations, fields are case sensitive   
---
### execution 
* execute into local mode
```
pig -x local
pig -x local <filename>
```
* execute on cluster using mapreduce
```
pig -x mapreduce
pig -x mapreduce <filename>
```
### building blocks
associations with SQL
```
# just a value, like "hello", 15...
cell  -> field
# (15, 4.2, "hello")
row   -> tuple
# { (15, 4.2, "first"), (16, 5.3,"second") }
table -> bag
```

### data types
```
map: [1#red, 2#yellow, 3#green]
bag: (1, red), (2, yellow), (3, green)
tuple: (1,red)
atom: int, long, float, double, chararray, bytearray
```
### input output
loading data from file
```
/* load data from file and use semicolon delimiter, default delimiter = \t */
LOAD <path to file/folder> USING PigStorage(';') AS (userId: chararray, timestamp: long ); 
LOAD <path to file/folder> AS (userId: chararray, timestamp: long ); -- tab will be used as delimiter for default loader - PigStorage('\t')
LOAD <path to file/folder> USING TextLoader() -- tuple with one value - single line of text
LOAD <path to file/folder> USING JsonLoader() -- json format
LOAD <path to file/folder> USING BinStorage() -- for internal using 
```
[custom delimiter implementation](https://stackoverflow.com/questions/26354949/how-to-load-files-with-different-delimiter-each-time-in-piglatin#26356592)

saving data into file
> operation will fail, if directory exists
```
STORE <var name> INTO <path to directory>
STORE <var name> INTO <path to directory> USING PigStorage() -- default saver
STORE <var name> INTO <path to directory> USING BinStorage()
STORE <var name> INTO <path to directory> USING PigDump()
STORE <var name> INTO <path to directory> USING JsonStorage()
```
### CLI
using command line parameters into PigLating script
```
./pig -param dir='/opt/data/import' myscript.pig
```
script with used parameter from command line
```
/* inside script it is looks like */
employee = LOAD "$dir/employee.csv" as PigStorage("\t")
```
---
using one parameter file to combine all parameters
custom_parameters.values
```
dir = '/opt/data/import'
user = 'Michael'
```
command line
```
./pig -param_file custom_parameters.values myscript.org
```
### operators
describe variable, print type
```
DESCRIBE <var name>;
```
---
explanation of execution ( logical, physical, MapReduce)
```
EXPLAIN <var name>;
```
---
print content of the variable
```
DUMP <var name>;
```
---
history of the variable, variable transformation steps
```
ILLUSTRATE <var name>;
```
---
group into new variable
```
{bag} = GROUP <var name> by <field name>;
# result will be
# {field_value, (tuple of values)}
```
example
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)

groupped_data = group data by f1;
dump groupped_data;
( 1, {(1,2,3)} )
( 4, {(4,5,6), (4,3,2)} )
( 7, {(7,8,9)} )

```
---
cogroup - multiply grouping
```
{bag} = COGROUP <var name> by <field name>, <var name> by <field name>;
```
example
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)

comments = load 'comments.csv' as (id: int, name: chararray);
dump comments;
> (1, "first")
> (2, "second")
> (3, "third")
> (4, "fourth")

groupped_data = cogroup data by f1, comments by id;
dump groupped_data;
( 1, {(1,2,3)}, {(1, "first")} )
( 4, {(4,5,6), (4,3,2)}, {(4, "fourth")} )
( 7, {(7,8,9)} )

```
---
map value one-by-one, walk through variable, foreach with outer relation 
```
{bag} = FOREACH <var name> GENERATE <var field>, FUNCTION(<var field>);  
```
example
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)
```
extension = FOREACH data f1,f2,f3, f1+f2 as s12:int, f2+f3 as s23;
```
> (1,2,3,3,5)
> (4,5,6,9,11)
> (7,8,9,15,17)
> (4,3,2,7,5)
```
---
foreach with a grouped relation
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)

groupped_data = group data by f1;
dump groupped_data;
( 1, {(1,2,3)} )
( 4, {(4,5,6), (4,3,2)} )
( 7, {(7,8,9)} )

/* name of the first field from previous dataset will be 'group' */
FOREACH groupped_data GENERATE group, data.f2, data.f3
>(1, { (2) }, { (3) })
>(4, { (5), (3) }, { (6),(2) })
>(7, { (8) }, { (9) })

```
---
foreach nested block
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)

comments = load 'comments.csv' as (id: int, name: chararray);
dump comments;
> (1, "first")
> (2, "second")
> (3, "third")
> (4, "fourth")

result = FOREACH comments{
	filtered_data = filter data by f1<=4;
	generate name, filtered_data.f2, filtered_data.f3;
--  generate name, filtered_data.$1, filtered_data.$2;
};
dump result;

> ("first", 2,3)
> ("second")
> ("third")
> ("fourth", 5, 6)
> ("fourth", 3, 2)
```

accessible nested functions:
* DISTINCT
* FILTER {var} BY {field} MATCHES
* LIMIT ( like SQL limit )
* ORDER {var} BY {field}
---
# functions 
* FLATTEN - flat map for tuples ( and nested elements )
```
dump data
(1,(4,7))
(2,(5,8))
(3,(6,9))

FOREACH data generate $0, flatten($1);
>(1,4,7)
>(2,5,8)
>(3,6,9)

will work even for:
(1,{(4),(7)}) -> (1,4,7)
```

* AVG
average of the number values in a single column
* CONCAT
concatanation of two columns
* TOKENIZE
split string and return tuple of words
* COUNT
count number of elements, require GROUP BY/GROUP ALL
* COUNT_STAR
count number of elments into a bag
* DIFF
compare two fields in tuple
* IsEmpty
is field empty/null
* MAX/MIN/SUM
maximum/minimum/summarize value, require GROUP BY/GROUP ALL
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2)
> (4,5)
> (7,8)
> (4,3)

groupped_data = group data by f1;
dump groupped_data;
( 1, {(1,2)} )
( 4, {(4,5), (4,3)} )
( 7, {(7,8)} )

FOREACH groupped_data GENERATE group, MAX(groupped_data.f2)
> (1, 2)
> (4, 5)
> (7, 8)
```
* SUM
* ....
---
filter by condition
```
FILTER <var name> BY <field name> operation;
FILTER <var name> BY <field name> MATCHES <regexp>;
```
example:
```
data = LOAD <path to file/folder> USING PigStorage(';') AS (userId: chararray, timestamp: long );
filtered_data = FILTER data BY timestamp is not null
```
* ABS
absolute value
* CEIL
round to the nearest up integer
* STRSPLIT
split around Regular Expression
* REGEX_EXTRACT/REGEX_EXTRACT_ALL
extract elements according given Regular Expression
* SUBSTRING
substring of the given string
* REPLACE
replace existing characters with new one/sequence
* TOTUPLE
* TOBAG
* TOMAP

* MAPREDUCE
execute external job
* STREAM
send data to external script or program
* UDF DEFINE 
assign alias to user defined function
* UDF REGISTER 
register jar with UDF
* fs
invoke FSShell command ( ls, mkdir, )

---
order data
```
ordered_data = ORDER data BY timestamp DESC
```
---
distinct
```
dump data;
>(1,4,7)
>(2,5,8)
>(2,5,8)
>(2,5,8)

DISTINCT data;

>(1,4,7)
>(2,5,8)

```
---
union
```
dump data;
>(1,4,7)
>(2,5,8)

dump data2
>(2,5,8)
>(2,5,8)

UNION data, data2;

>(1,4,7)
>(2,5,8)
>(2,5,8)
>(2,5,8)

```
---
split data to different dataset using conditions
```
dump data
>(1,4,7)
>(2,5,8)
>(3,6,9)

SPLIT data INTO data_small IF $0<=1, data_big IF $0>1;

DUMP data_small;
>(1,4,7)

DUMP data_big;
>(2,5,8)
>(3,6,9)
```
---
cross values
```
DUMP data1;
> (1,2,3)
> (4,5,6)

DUMP data2;
> (7,8)
> (9,10)

CROSS data1, data2;
> (1,2,3, 7,8)
> (1,2,3, 9,10)
> (4,5,6, 7,8)
> (4,5,6, 9,10)
```
---
join variables, inner join, outer join for variables
```
posts = LOAD '/data/user-posts.txt' USING PigStorage(',') AS (user:chararray, post:chararray, date:timestamp);
likes = LOAD '/data/user-likes.txt' USING PigStorage(',') AS (user_name:chararray, user_like:chararray, like_date:long);
JOIN posts BY user, likes BY user_name;
JOIN posts BY user LEFT OUTER, likes BY user_name;
JOIN posts BY user FULL OUTER, likes BY user_name;
> result will be: ( posts::user, posts::post, posts::date, likes:user_name, likes:user_like, likes:like_date )
-- the same but with reference using position of the field
JOIN posts BY $0, likes BY $0;
```
example
```
DUMP data1;
> (1,2,3)
> (1,2,4)
> (2,3,4)
> (2,3,5)

DUMP data2;
> ("first", 1)
> ("second", 2)

JOIN data1 BY $0, data2 BY, $1
> (1,2,3, "first", 1)
> (1,2,4, "first", 1)
> (2,3,4, "second", 2)
> (2,3,5, "second", 2)
```

[Pig UDF](https://pig.apache.org/docs/latest/udf.html)
