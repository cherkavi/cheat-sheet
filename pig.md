## Pig
free data structure ( Hive - fixed data structure )
only function names are case sensitive 
execute into local mode
```
pig -x local
pig -x local <filename>
```
load data from file
```
/* load data from file and use semicolon delimiter, default delimiter = \t */
LOAD <path to file> USING PigStorage(';') AS (userId: chararray, timestamp: long ); 
LOAD <path to file> AS (userId: chararray, timestamp: long ); -- space will be used as delimiter 
LOAD <path to file> USING TextLoader() -- tuple with one value - single line of text
LOAD <path to file> USING JsonLoader() -- json format
LOAD <path to file> USING BinStorage() -- for internal using 
```
[custom delimiter implementation](https://stackoverflow.com/questions/26354949/how-to-load-files-with-different-delimiter-each-time-in-piglatin#26356592)
[Pig UDF](https://pig.apache.org/docs/latest/udf.html)

save data
```
STORE <var name> INTO <path to file>
```
describe variable, print type
```
DESCRIBE <var name>;
```
print content of the variable
```
DUMP <var name>;
```
group into new variable
```
{bag} = GROUP <var name> by <field name>;
```
history of the variable, variable transformation steps
```
ILLUSTRATE <var name>;
```
map value one-by-one, walk through variable 
```
{bag} = FOREACH <var name> GENERATE <var field>, FUNCTION(<var field>);  
```
data types
```
map: [1#red, 2#yellow, 3#green]
bag: (1, red), (2, yellow), (3, green)
tuple: (1,red)
atom: int, long, float, double, chararray, bytearray
```

functions 
```
TOKENIZE - split
FLATTEN, - flat map
COUNT, 
SUM,....
```
filter by condition
```
FILTER <var name> BY <field name> operation;
FILTER <var name> BY <field name> MATCHES <regexp>;
```
join variables, inner join, outer join for variables
```
posts = LOAD '/data/user-posts.txt' USING PigStorage(',') AS (user:chararray, post:chararray, date:timestamp);
likes = LOAD '/data/user-likes.txt' USING PigStorage(',') AS (user_name:chararray, user_like:chararray, like_date:long);
JOIN posts BY user, likes BY user_name;
JOIN posts BY user LEFT OUTER, likes BY user_name;
JOIN posts BY user FULL OUTER, likes BY user_name;
> result will be: ( posts::user, posts::post, posts::date, likes:user_name, likes:user_like, likes:like_date )
```

