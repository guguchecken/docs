# Bug

## 填写步骤

1. 必填，填写issue标题。标题应该能够很好的概括发生的错误，并且应该尽量的简短

2. 必填，仔细检查此前是否已经有相同问题的issue存在。若存在则可以参考该issue的解决方法，此次issue结束；若不存在则勾选下面的选项，进行此次issue的详情填写。

3. 必填，填写出现问题的相关软件硬件平台信息。应尽可能详细的填写相关的信息，能够影响产品运行的因素应该尽量的详细。其中，version或commit id部分至少填写对应`version`；Hardware parameters部分至少填写`处理器相关信息`；OS type应填写对应操作系统名称；若是Windows或者Mac OS，则应该补充其`系统版本号`，若是Linux，则应该补充其`发行版本名称以及版本号`；Others部分应填写一些可能会对产品运行产生影响的附加信息。

4. 必填，填写问题表现形式。此处应该填写产生问题的相关命令或者相关函数以及对应的错误信息。

5. 选填，填写正常程序运行应该展现的结果。此处应该填写程序正常运行，符合逻辑的结果表现，应只针对步骤三中出现错误的地方进行修改展示。

6. 选填，填写复现步骤。此处应该填写产生该问题的具体步骤。步骤应该尽量最少。

7. 选填，填写附加信息。此处应该填写一些能够帮助开发人员复现问题，定位问题以及修复问题的一些信息。

## 例子

### [Bug]: The result of group by from subquery not correct

### Is there an existing issue for the same bug?

- [x] I have checked the existing issues.

### Environment

~~~bash
- Version or commit-id (e.g. v0.1.0 or 8b23a93): 0.4.0
- Hardware parameters: Intel(R) Xeon(R) Platinum 8255C CPU @ 2.50GHz
- OS type: Linux Ubuntu 18.04.6 LTS
- Others: 
~~~

### Actual Behavior

~~~mysql
mysql> select * from t1;
+------+------+------+------+-----------+--------------+------+--------------+----------------+------------+---------------------+
| id | ti | si | bi | fl | dl | de | ch | vch | dd | dt |
+------+------+------+------+-----------+--------------+------+--------------+----------------+------------+---------------------+
| 1 | 1 | 4 | 3 | 1113.3199 | 111332.0000 | 1113 | hello | subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 2 | 2 | 5 | 2 | 2252.0500 | 225205.0000 | 2252 | bye | sub query | 2022-04-28 | 2022-04-28 22:40:11 |
| 3 | 6 | 6 | 3 | 3663.2100 | 366321.0000 | 3663 | hi | subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 4 | 7 | 1 | 5 | 4715.2202 | 471522.0000 | 4715 | good morning | my subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 5 | 1 | 2 | 6 | 51.2600 | 5126.0000 | 51 | byebye | is subquery? | 2022-04-28 | 2022-04-28 22:40:11 |
| 6 | 3 | 2 | 1 | 632.1000 | 6321.0000 | 632 | good night | maybe subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 7 | 4 | 4 | 3 | 7443.1099 | 744311.0000 | 7443 | yes | subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 8 | 7 | 5 | 8 | 8758.0000 | 875800.0000 | 8758 | nice to meet | just subquery | 2022-04-28 | 2022-04-28 22:40:11 |
| 9 | 8 | 4 | 9 | 9849.3115 | 9849312.0000 | 9849 | see you | subquery | 2022-04-28 | 2022-04-28 22:40:11 |
+------+------+------+------+-----------+--------------+------+--------------+----------------+------------+---------------------+
9 rows in set (0.08 sec)

mysql> select id,min(ti) from (select * from t1) sub group by id;
Empty set (0.02 sec)
~~~

### Expected Behavior

~~~mysql
mysql> select id,min(ti) from (select * from t1) sub group by id;
+------+---------+
| id | min(ti) |
+------+---------+
| 1 | 1 |
| 2 | 2 |
| 3 | 6 |
| 4 | 7 |
| 5 | 1 |
| 6 | 3 |
| 7 | 4 |
| 8 | 7 |
| 9 | 8 |
+------+---------+
9 rows in set (0.00 sec)
~~~

### Steps to Reproduce

~~~mysql
drop table if exists t1;
create table t1 (id int,ti tinyint unsigned,si smallint,bi bigint unsigned,fl float,dl double,de decimal,ch char(20),vch varchar(20),dd date,dt datetime);
insert into t1 values(1,1,4,3,1113.32,111332,1113.32,'hello','subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(2,2,5,2,2252.05,225205,2252.05,'bye','sub query','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(3,6,6,3,3663.21,366321,3663.21,'hi','subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(4,7,1,5,4715.22,471522,4715.22,'good morning','my subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(5,1,2,6,51.26,5126,51.26,'byebye',' is subquery?','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(6,3,2,1,632.1,6321,632.11,'good night','maybe subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(7,4,4,3,7443.11,744311,7443.11,'yes','subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(8,7,5,8,8758.00,875800,8758.11,'nice to meet','just subquery','2022-04-28','2022-04-28 22:40:11');
insert into t1 values(9,8,4,9,9849.312,9849312,9849.312,'see you','subquery','2022-04-28','2022-04-28 22:40:11');


select id,min(ti) from (select * from t1) sub group by id;
~~~

### Additional information

*No response*

# Documentation 

## 填写步骤

1. 仔细填写标题。应在标题中指出出现问题的文档类型以及出现的问题类型。
2. 仔细检查此前是否已经有相同问题的issue存在。若存在则可以参考该issue的解决方法，此次issue结束；若不存在则勾选下面的选项，进行此次issue的详情填写。
3. 填写问题详情。此处应该填写出现问题的文档路径，文档名称，错误的行数（或者所属的标题）以及错误详情。
4. 填写附加信息。在此处应该填写能够帮助定位错误以及修复错误的相关资料，用以帮助文档开发人员修复错误。

## 例子

### [Docs]: Typo in the architecture picture

### Is there an existing issue about documentation?

- [x] I have checked the existing issues.

### Describe the issue

In the architecture picture in the README.md file

```html
"JIT complication" should be "JIT compilation"?
```

### Additional information

~~~html
complication:  sth that adds new difficulties; or new development of an illness
compilation: something that is compiled (as into a single book or file)
~~~



# Enhancement

## 填写流程

1. 必填，仔细填写标题。应在标题中概要说明此次issue所需要的提升。
2. 必填，仔细检查此前是否已经有相同问题的issue存在。若存在则此次issue结束；若不存在则勾选下面的选项，进行此次issue的详情填写。
3. 必填，填写增强需求详情。应在此处填写本次所需要增加或者改进的功能增强需求。
4. 选填，填写为什么需要此次增强。此处应该填写增加此次增强的原因。
5. 选填，填写附加信息。此处应该填写能够帮助开发人员进行判定此次issue提出的增强是否可行以及是否其重要程度。

## 例子

### [Enhancement]: Func month,weekday need to support column type of char,varchar

### Is there an existing issue for enhancement?

- [x] I have checked the existing issues.

### What would you like to be added ?

~~~mysql
mysql> select month(c),weekday(vc) from t1;
ERROR 1105 (HY000): unknown error:'' not yet implemented for VARCHAR

It should be like:

mysql> select month(c),weekday(vc) from t1;
+----------+-------------+
| month(c) | weekday(vc) |
+----------+-------------+
|       12 |           3 |
|       12 |           3 |
|        2 |           0 |
|        2 |           0 |
|        2 |           0 |
|        3 |           2 |
|        4 |           3 |
|        5 |           6 |
|        6 |           3 |
|        7 |           6 |
|        8 |           6 |
|        9 |           3 |
|       10 |           6 |
|       11 |           6 |
|       12 |           6 |
|     NULL |        NULL |
+----------+-------------+
16 rows in set (0.00 sec)

~~~

### Why is this needed ?

This function greatly improves the user experience.

### Additional information

*No response*

# Feature request

## 填写流程

1. 必填，仔细填写标题。应在标题中概要说明此次issue所需要的功能，应该使用祈使语气。
2. 选填，填写本次功能需求是否与一些问题相关联。若本次功能需求与某些问题相关，则应该在本部分对问题进行简要明确的描述；若不相关则跳过此部分。
3. 必填，填写本次功能需求的详情。在此部分应该详细描述所需求的功能的输入参数，函数名称，输出参数，实现原理等等。
4. 选填，填写对于此次功能需求已知的实现方法。若有一些已知的实现方法，应填写在此处，帮助开发人员迅速开发功能；若没有，则跳过此部分。
5. 选填，填写相关文档、用例、迁移策略等信息。填写相关信息，帮助开发人员在开发过程中规范，优化代码结构。
6. 选填，填写附加信息。此处应该填写能够帮助开发人员进行判定此次issue提出的功能需求是否可行以及是否其重要程度。

## 例子

### [Feature Request]: Builtin Function Find_in_set()

### Is there an existing issue for the same feature request?

- [x] I have checked the existing issues.

### Is your feature request related to a problem?

~~~mysql
Showcase SQL requires support of this function.


SELECT c.workorder_no,d.nuc_id, e.station_name,a.action_time, a.action_detail,
	CASE
				WHEN FIND_IN_SET('U360CEN', SUBSTRING( d.nuc_id, 1, 7 ) ) THEN 'UCL360C'
				WHEN FIND_IN_SET('U360EN', SUBSTRING( d.nuc_id, 1, 6 )  ) THEN 'UCL360'
				WHEN FIND_IN_SET('U360CN', SUBSTRING( d.nuc_id, 1, 6 )  ) THEN 'UCL360' ELSE 'UCL360'
				END AS number FROM mfg_tracking a
				INNER JOIN vw_mfg_tracking_hardwares b ON a.workorder_id = b.workorder_id
				AND a.device_id = b.device_id
				INNER JOIN mfg_work_orders c ON a.workorder_id = c.workorder_id
				INNER JOIN mfg_devices d ON a.device_id = d.device_id
				INNER JOIN mfg_work_stations e ON a.station_id = e.station_id where 1=1 
~~~

### Describe the feature you'd like

~~~html
func name: FIND_IN_SET
input: str,strlist
output: index(type: int)

function FIND_IN_SET(str,strlist) need returns a value in the range of 1 to N if the string str is in the string list strlist consisting of N substrings. A string list is a string composed of substrings separated by , characters.For example:

mysql> SELECT FIND_IN_SET('b','a,b,c,d');
=====> 2
~~~

### Describe implementation you've considered

Follow the syntax of MySQL8.0

[FIND_IN_SET(str,strlist)](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_find-in-set)

> Returns a value in the range of 1 to N if the string str  is in the string list strlist consisting of N substrings. A string list  is a string composed of substrings separated by , characters. If the  first argument is a constant string and the second is a column of type  SET, the FIND_IN_SET() function is optimized to use bit arithmetic.  Returns 0 if str is not in strlist or if strlist is the empty string.  Returns NULL if either argument is NULL. This function does not work  properly if the first argument contains a comma (,) character.

### Documentation, Adoption, Use Case, Migration Strategy

Please refer to the [tutorial](https://docs.matrixorigin.io/0.4.0/MatrixOne/Contribution-Guide/Tutorial/develop_builtin_functions/) about how to develop a builtin function.

### Additional information

*No response*

# General question

## 填写流程

1. 必填，填写issue标题。标题应该能够很好的概括遇到的问题，并且应该尽量的简短
2. 必填，填写遇到的问题详细信息。在此部分，应该尽可能详细的描述遇到的问题，以及已经查阅过并且记录下来的对于问题有所帮助的文档，网页等信息。能够使开发人员迅速的明白问题以及尽可能快的找到解决方法。

## 例子

### [Question]: Which one will be the result of plan2

If the  previous templates don't fit with what you'd like to report or ask,  please use this general question template to file issue.

Before asking a question, make sure you have:

- Searched open and closed [GitHub issue](https://github.com/matrixorigin/matrixone/issues)
- Read the documentation:
- [MatrixOne Readme](https://github.com/matrixorigin/matrixone)
- [MatrixOne Doc](https://docs.matrixorigin.io)

### Describe your problem

~~~html
Regarding the computing between two date value, database acts differetly. Take this as example:
select cast('2022-02-01' as date)-cast('2022-01-01' as date);

In MySQL, the result is 100.
In PG, the result is 31.
In SQLServer and Oracle, the result is not supported directly. But using to_date function, the result is 31.
~~~



# Performance question

## 填写流程

1. 必填，填写issue标题。标题应该能够很好的概括发生的性能问题，并且应该尽量的简短

2. 必填，仔细检查此前是否已经有相同问题的issue存在。若存在则可以参考该issue的解决方法，此次issue结束；若不存在则勾选下面的选项，进行此次issue的详情填写。

3. 必填，填写出现问题的相关软件硬件平台信息。应尽可能详细的填写相关的信息，能够影响产品运行的因素应该尽量的详细。其中，version或commit id部分至少填写对应`version`；Hardware parameters部分至少填写`处理器相关信息`；OS type应填写对应操作系统名称；若是Windows或者Mac OS，则应该补充其`系统版本号`，若是Linux，则应该补充其`发行版本名称以及版本号`；Others部分应填写一些可能会对产品运行产生影响的附加信息。
4. 必填，填写出现的性能问题的详细信息。在此处应该填写在使用过程中遇到的性能问题的详情以及期望的性能表现，若知道已经存在的性能提升方法，则应该也填写在此处。
5. 选填，填写附加信息。此处应该填写能够帮助开发人员进行判定此次issue提出的功能需求是否可行以及是否其重要程度。

## 例子

### [Performance]: support parallel vectorized query

### Is there an existing issue for the same bug?

- [x] I have checked the existing issues.

### Environment

~~~html
- Version or commit-id (e.g. v0.1.0 or 8b23a93): 0.4.0
- Hardware parameters: Intel(R) Xeon(R) Platinum 8255C CPU @ 2.50GHz
- OS type: Linux Ubuntu 18.04.6 LTS
- Others: go version: 1.18.1
~~~

### Details of Performance

~~~html
now, MO's simple query sql (a query without aggregate function and join)
like "select * from t1 where a = 100;"
is executed in only one thread.Its select speed is too slow when select-size is large.
And some condition (like limit 10) can be push down, but MO didn't do it.

we should push down more condition for reducing the size of the execute-data,
and run a simple query in parallel way if necessary.
~~~

### Additional information

One way that I think might work is:

~~~html
Add 5 new operators (merge-top, merge-limit, merge-order, merge-dedup, merge-offset)

change pipeline-build code for simple select (function compileQ)
~~~



# Refactoring request

## 填写流程

1. 必填，填写issue标题。标题应该能够很好的概括需要重构的代码部分或者重构代码的目的，并且应该尽量的简短
2. 必填，填写提出此次重构的原因。在此部分应该描述此次重构的详细原因，以及重构代码之后所会带来的代码质量，运行性能等等方面的可预见的提升。
3. 必填，填写此次代码重构的可行的方法。此处应该填写用于代码重构的最佳解决方法，用于帮助开发人员进行代码重构工作。
4. 选填，填写可用于此次代码重构的替代方法。用于帮助开发人员开阔思路，发现更多代码重构的好的方法。
5. 选填，填写附加信息。此处应该填写能够帮助开发人员进行判定此次issue提出的需求是否可行以及是否其重要程度。

## 例子

### [Refactoring]: Golang generics, to reduce code dup

### Why do you want to refactor this code?

Prior to version 1.18, Go did not support generic programming, which resulted in a lot of duplication in our code, reducing brevity and readability

### Describe the solution you'd like

Use the generics feature provided by Golang since version 1.18 to refactor functions with different input types for the same function

### Describe alternatives you've considered

*No response*

### Additional information

[Golang Generics](https://go.dev/doc/tutorial/generics)