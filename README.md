## Welcome to My DataBase Internals Page.

## Storage:

DataBase data storage could be either row or clomn wise storage model. Each version has its own advantage.

## MonetDB

MonetDB is an open-source column-oriented database management system.

#[Monetdb java client program](https://www.monetdb.org/Documentation/Manuals/SQLreference/Programming/JDBC)

#[Monet User guid](https://www.monetdb.org/book/export/html/202)

### MonetDB MAL
MonetDB supports SQl(Standard Query Language). Every SQL will be translated into MonetDB Assembly Lnaguage, which is low language to apply relational algebra efficiently.



```markdown

sql>select * from test where a = 2;
+------+------+------+------+------+
| a    | b    | c    | d    | e    |
+======+======+======+======+======+
|    2 |    2 | null | null | null |
+------+------+------+------+------+
1 tuple
sql>select count(*) from test;
+----------+
| L3       |
+==========+
| 13901600 |
+----------+
1 tuple
sql>
```
Updated the column c of table test using MAL instrctions. See the amazing performance from monetdb on coulumn updates

```markdown
>>time cat mal.ins |  mclient  -dmy-first-db -l mal
real	0m0.226s
user	0m0.008s
sys	0m0.000s

````

MAL instruction


```markdown
>> cat mal.ins 

sql.init();
s:int := sql.mvc();
test_a:bat[:int] := sql.bind(s, "sys", "test","a",0);
test_b:bat[:int] := sql.bind(s, "sys", "test","b",0);


test_oid:bat[:oid] := sql.tid(s, "sys", "test");

new_column := batcalc.+(test_a, test_b);
sql.update(s, "sys", "test", "c", test_oid, new_column); 
```
Verify the test results using SQL

```markdown
sql>select * from test where a = 2;
+------+------+------+------+------+
| a    | b    | c    | d    | e    |
+======+======+======+======+======+
|    2 |    2 |    4 | null | null |
+------+------+------+------+------+
1 tuple

```
