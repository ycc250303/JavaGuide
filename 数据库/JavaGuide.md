# SQL基础语法

## SQL语法

### SQL语法结构

<pre class="vditor-reset" placeholder="" contenteditable="true" spellcheck="false"><p data-block="0"><img src="https://file+.vscode-resource.vscode-cdn.net/d%3A/GitHub/ycc/JavaGuide/%E6%95%B0%E6%8D%AE%E5%BA%93/image/JavaGuide/1760313178746.png" alt="1760313178746"/></p></pre>

SQL 语法结构包括：

* **`子句`** - 是语句和查询的组成成分。（在某些情况下，这些都是可选的。）
* **`表达式`** - 可以产生任何标量值，或由列和行的数据库表
* **`谓词`** - 给需要评估的 SQL 三值逻辑（3VL）（true/false/unknown）或布尔真值指定条件，并限制语句和查询的效果，或改变程序流程。
* **`查询`** - 基于特定条件检索数据。这是 SQL 的一个重要组成部分。
* **`语句`** - 可以持久地影响纲要和数据，也可以控制数据库事务、程序流程、连接、会话或诊断。

## 子查询

子查询常用在 `WHERE` 子句和 `FROM` 子句后边：

```sql
select column_name [, column_name ]
from   table1 [, table2 ]
where  column_name operator
    (select column_name [, column_name ]
    from table1 [, table2 ]
    [where])
```

```sql
select column_name [, column_name ]
from (select column_name [, column_name ]
      from table1 [, table2 ]
      [where]) as temp_table_name
where  condition
```

## 连接

### **`join ON` 和 `WHERE` 的区别** 

* 连接表时，SQL 会根据连接条件生成一张新的临时表。`ON` 就是连接条件，它决定临时表的生成。
* `WHERE` 是在临时表生成以后，再对临时表中的数据进行过滤，生成最终的结果集，这个时候已经没有 JOIN-ON 了。

所以总结来说就是： **SQL 先根据 ON 生成一张临时表，然后再根据 WHERE 对临时表进行筛选** 。

## 组合

`UNION` 基本规则：

* 所有查询的列数和列顺序必须相同。
* 每个查询中涉及表的列的数据类型必须相同或兼容。
* 通常返回的列名取自第一个查询。

`JOIN` vs `UNION`：

* `JOIN` 中连接表的列可能不同，但在 `UNION` 中，所有查询的列数和列顺序必须相同。
* `UNION` 将查询之后的行放在一起（垂直放置），但 `JOIN` 将查询之后的列放在一起（水平放置），即它构成一个笛卡尔积。

## 索引

## 约束

## 事务处理

不能回退 `SELECT` 语句，回退 `SELECT` 语句也没意义；也不能回退 `CREATE` 和 `DROP` 语句。

 **MySQL 默认是隐式提交** ，每执行一条语句就把这条语句当成一个事务然后进行提交。当出现 `START TRANSACTION` 语句时，会关闭隐式提交；当 `COMMIT` 或 `ROLLBACK` 语句执行后，事务会自动关闭，重新恢复隐式提交。

指令：

* `START TRANSACTION` - 指令用于标记事务的起始点。
* `SAVEPOINT` - 指令用于创建保留点。
* `ROLLBACK TO` - 指令用于回滚到指定的保留点；如果没有设置保留点，则回退到 `START TRANSACTION` 语句处。
* `COMMIT` - 提交事务。

## 权限控制
