\1. SQL优化的原则是：将一次操作需要读取的BLOCK数减到最低,即在最短的时间达到最大的数据吞吐量。

调整不良SQL通常可以从以下几点切入：

-  检查不良的SQL，考虑其写法是否还有可优化内容
-  检查子查询 考虑SQL子查询是否可以用简单连接的方式进行重新书写
-  检查优化索引的使用
-  考虑数据库的优化器



\2. 避免出现SELECT * FROM table 语句，要明确查出的字段。

\3. 在一个

SQL语句

中，如果一个where条件过滤的数据库记录越多，定位越准确，则该where条件越应该前移。

\4. 查询时尽可能使用索引覆盖。即对SELECT的字段建立复合索引，这样查询时只进行索引扫描，不读取数据块。

\5. 在判断有无符合条件的记录时建议不要用SELECT COUNT （*）和select top 1 语句。

\6. 使用内层限定原则，在拼写

SQL语句

时，将查询条件分解、分类，并尽量在

SQL语句

的最里层进行限定，以减少数据的处理量。

\7. 应绝对避免在order by子句中使用表达式。

\8. 如果需要从关联表读数据，关联的表一般不要超过7个。

\9. 小心使用 IN 和 OR，需要注意In集合中的数据量。建议集合中的数据不超过200个。

\10. <> 用 < 、 > 代替，>用>=代替，<用<=代替，这样可以有效的利用索引。

\11. 在查询时尽量减少对多余数据的读取包括多余的列与多余的行。

\12. 对于复合索引要注意，例如在建立复合索引时列的顺序是

F1

，F2，F3，则在where或order by子句中这些字段出现的顺序要与建立索引时的字段顺序一致，且必须包含第一列。只能是

F1

或

F1

，F2或F1，F2，F3。否则不会用到该索引。

\13. 多表关联查询时，写法必须遵循以下原则，这样做有利于建立索引，提高查询效率。格式如下select sum（table1.je） from table1 table1, table2 table2, table3 table3 where (table1的等值条件（=）) and (table1的非等值条件) and (table2与table1的关联条件) and (table2的等值条件) and (table2的非等值条件) and (table3与table2的关联条件) and (table3的等值条件) and (table3的非等值条件)。

注:关于多表查询时from 后面表的出现顺序对效率的影响还有待研究。

\14. 子查询问题。对于能用连接方式或者视图方式实现的功能，不要用子查询。例如：select name from customer where customer_id in ( select customer_id from order where money>1000)。应该用如下语句代替：select name from customer inner join order on customer.customer_id=order.customer_id where order.money>100。

\15. 在WHERE 子句中，避免对列的四则运算，特别是where 条件的左边，严禁使用运算与函数对列进行处理。比如有些地方 substring 可以用like代替。

\16. 如果在语句中有not in（in）操作，应考虑用not exists（exists）来重写,最好的办法是使用外连接实现。

\17. 对一个业务过程的处理，应该使事物的开始与结束之间的时间间隔越短越好，原则上做到数据库的读操作在前面完成，数据库写操作在后面完成，避免交叉。

\18. 请小心不要对过多的列使用列函数和order by,group by等，谨慎使用disti软件开发。

\19. 用union all 代替 union，数据库执行union操作，首先先分别执行union两端的查询，将其放在临时表中，然后在对其进行排序，过滤重复的记录。

当已知的

业务逻辑

决定query A和query B中不会有重复记录时，应该用union all代替union，以提高查询效率。