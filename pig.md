## Pig
free data structure ( Hive - fixed data structure )
function names, names of relations, fields are case sensitive 
---
execute into local mode
```
pig -x local
pig -x local <filename>
```
data types
```
map: [1#red, 2#yellow, 3#green]
bag: (1, red), (2, yellow), (3, green)
tuple: (1,red)
atom: int, long, float, double, chararray, bytearray
```
load data from file
```
/* load data from file and use semicolon delimiter, default delimiter = \t */
LOAD <path to file/folder> USING PigStorage(';') AS (userId: chararray, timestamp: long ); 
LOAD <path to file/folder> AS (userId: chararray, timestamp: long ); -- tab will be used as delimiter for default loader - PigStorage('\t')
LOAD <path to file/folder> USING TextLoader() -- tuple with one value - single line of text
LOAD <path to file/folder> USING JsonLoader() -- json format
LOAD <path to file/folder> USING BinStorage() -- for internal using 
```
[custom delimiter implementation](https://stackoverflow.com/questions/26354949/how-to-load-files-with-different-delimiter-each-time-in-piglatin#26356592)
[Pig UDF](https://pig.apache.org/docs/latest/udf.html)
---
save data
> operation will fail, if directory exists
```
STORE <var name> INTO <path to directory>
STORE <var name> INTO <path to directory> USING PigStorage() -- default saver
STORE <var name> INTO <path to directory> USING BinStorage()
STORE <var name> INTO <path to directory> USING PigDump()
STORE <var name> INTO <path to directory> USING JsonStorage()
```
---
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
---
describe variable, print type
```
DESCRIBE <var name>;
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
```
example
```
data = load 'data.csv' as (f1:int, f2:int, f3:int);
dump data;
> (1,2,3)
> (4,5,6)
> (7,8,9)
> (4,3,2)

groupped_data = group data f1;
dump groupped_data;
( 1, {(1,2,3)} )
( 4, {(4,5,6), (4,3,2)} )
( 7, {(7,8,9)} )

```
---
map value one-by-one, walk through variable 
```
{bag} = FOREACH <var name> GENERATE <var field>, FUNCTION(<var field>);  
```
---
functions 
```
TOKENIZE - split
FLATTEN, - flat map
COUNT, 
SUM,....
```
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
---
order data
```
ordered_data = ORDER data by timestamp DESC
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

