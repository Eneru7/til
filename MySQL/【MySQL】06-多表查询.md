# 6.多表查询

多表查询：也称为关联查询，指两个或更多个表一起完成查询操作。

**前提条件**：这些一起查询的表之间是有关系的（一对一、一对多），它们之间一定是==有关联字段==，这个关联字段可能建立了外键，也可能没有建立外键。比如：员工表和部门表，这两个表依靠“部门编号”进行关联。

## 1.多表查询的一个案例

```mysql
SELECT COUNT(employee_id) FROM employees;
#输出107行
SELECT COUNT(department_id)FROM departments;
#输出27行

#案例：查询员工的姓名及其部门名称
SELECT last_name, department_name
FROM employees, departments;
#2889 rows in set (0.01 sec)
```

首先，last_name属于员工表employees的字段，department_name属于部门表departments的字段，如果想查询员工名字及该员工所属的部门，并且使用了上述的查询语句，将得到2889条数据，显然这不是我们想要的。上述多表查询中出现的==问题称为==：==笛卡尔积的错误==。

## 2.笛卡尔积（或称 交叉连接）的理解

笛卡尔乘积是一个**数学运算**。假设我有两个集合 X 和 Y，那么 X 和 Y 的笛卡尔积就是 X 和 Y 的==所有可能组合==，也就是第一个对象来自于 X，第二个对象来自于 Y 的所有可能。组合的个数即为两个集合中元素个数的乘积数。如下图所示：

![image-20220923121838349](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220923121838349.png)

SQL92中，笛卡尔积也称为交叉连接，英文是 CROSS JOIN 。在 SQL99 中也是使用 CROSS JOIN表示交叉连接。它的作用就是可以把任意表进行连接，即使这两张表不相关。在MySQL中如下情况会出现笛卡尔积：

* 省略多个表的连接条件（或关联条件）
* 连接条件（或关联条件）无效
* 所有表中的所有行互相连接

**为了避免笛卡尔积**， 可以在 WHERE 加入有效的连接条件，如下：

```mysql
SELECT table1.column, table2.column
FROM table1, table2
WHERE table1.column1 = table2.column2; #连接条件
```

按照上面规则，改写上面的例子：

```mysql
#案例：查询员工的姓名及其部门名称
SELECT last_name, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
#在表中有相同列时，在列名之前加上表名前缀。
```

## 3.多表查询分类讲解

### 3.1 等值连接vs非等值连接

#### ==等值连接==

上一节中查询员工的姓名及其部门名称的例子中where子句的判断条件就是在做等值判断，属于**等值连接**

```mysql
#案例：查询员工的姓名及其部门名称
SELECT last_name, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
#在表中有相同列时，在列名之前加上表名前缀。
```

**扩展**：表的别名

* 使用别名可以简化查询
* 列名前使用表名前缀可以提高查询效率

```mysql
SELECT e.employee_id, e.last_name, e.department_id,
d.department_id, d.location_id
FROM employees e , departments d
WHERE e.department_id = d.department_id;
```

> 需要注意的是，如果我们使用了表的别名，在查询字段中、过滤条件中就只能使用别名进行代替，
> 不能使用原有的表名，否则就会报错。

**扩展**：连接多个表

连接 n个表,至少需要n-1个连接条件。比如，连接三个表，至少需要两个连接条件。

#### ==非等值连接==

只要多表连接查询时，where子句的判断条件不是取等判断的，就是非等值连接



### 3.2 自连接vs非自连接

#### ==自连接==

下图是员工表的数据，这里在逻辑上，每个人都是员工，但不是所有员工都属于管理者（上级）。比如第一条数据的King就没有上级，第二条数据的Kochhar的上级id是100，即King。

![image-20220923123417740](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220923123417740.png)

要怎么查询出员工和该员工上级的名字呢？可以使用==自连接==，如下：

```mysql
SELECT CONCAT(worker.last_name ,' works for '
, manager.last_name)
FROM employees worker, employees manager
WHERE worker.manager_id = manager.employee_id ;
```

查询结果：

![image-20220923123911670](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220923123911670.png)

**自连接本质上查询的是同一张表，只是用取别名的方式虚拟成两张表以代表不同的意义。**

#### ==非自连接==

只要多表查询的不是出自同一张表，就认为是非自连接

### 3.3 内连接vs外连接

#### ==内连接==

现有员工表数据107条，其中有一条部门id为null，如下图：

![image-20220923125034540](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220923125034540.png)

现在查询员工id和对应部门名字，需要多表查询，语句如下：

```mysql
SELECT employee_id, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id;
```

结果如下：

![image-20220923125203979](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220923125203979.png)

一共查询出了106条数据，明显地，那条部门为null地员工数据不在结果集中。因此得出内连接特点：==根据where子句的判断条件，将满足条件的多表中的行数据进行合并，在结果集中不包含不满足条件的行数据。==

#### ==外连接==

需要说明的是，内连接和外连接是平常工作中使用最多的多表查询类型。上面我们知道了内连接的特点，不难推理出外连接的特点：==根据where子句的判断条件，将满足条件的多表中的行数据进行合并，在结果集中还包含不满足条件的行数据。==我们以两个表查询为例，形式可以分为左表和右表，根据这个**不满足条件的行数据**是在左表中，还是右表中，还是左右都有，外连接又细分为左外连接、右外连接和满外连接。

比如，还是上面员工和部门的例子，形式上认为员工表是左表，部门表为右表（左表还是右表看编写查询语句时，哪张表先出现那张表就是左表）。

员工数据中有一条数据部门id为null，用==左外连接==能查出107条数据，包括那条特殊员工数据（内连接能查出106条数据）。

有时，部门下并没有员工，用==右外连接==能查出107条数据，包括那条特殊部门数据。

满外连接即，内连接结果集加上左右两表中特殊的数据。

**注意**，外连接的语法在SQL92和SQL99中有不同的写法，在MySQL中不支持SQL92的写法，具体的外连接写法将在后续内容展开。

## 4.SQL99语法实现多表查询

### 4.1.基本语法

使用JOIN...ON子句创建连接的语法结构：

```mysql
SELECT table1.column, table2.column,table3.column
FROM table1
JOIN table2 ON table1 和 table2 的连接条件
JOIN table3 ON table2 和 table3 的连接条件
```

* 可以使用 ON 子句指定额外的连接条件。
* 这个连接条件是与其它条件分开的。
* ON 子句使语句具有更高的易读性。
* 关键字 JOIN、INNER JOIN、CROSS JOIN 的含义是一样的，都表示内连接。

### 4.2内连接 INNER JOIN

==语法==：

```mysql
SELECT 字段列表
FROM A表 INNER JOIN B表
ON 关联条件
WHERE 等其他子句;
```

例如：

```mysql
SELECT e.employee_id, e.last_name, e.department_id,
d.department_id, d.location_id
FROM employees e JOIN departments d
ON (e.department_id = d.department_id);
```

==注意==：一般使用内连接时，省略`INNER`，只写`JOIN`即表示内连接

### 4.3外连接 OUTER JOIN

#### 4.3.1左外连接 LEFT OUTER JOIN

==语法==：

```mysql
#实现查询结果是A
SELECT 字段列表
FROM A表 LEFT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

例子：

```mysql
SELECT e.last_name, e.department_id, d.department_name
FROM employees e
LEFT OUTER JOIN departments d
ON (e.department_id = d.department_id) ;
#结果集中将包含左表（employees）不满足匹配条件的数据
```

==注意==：一般使用左连接时，省略`OUTER`，只写`LEFT JOIN`即表示内连接

#### 4.3.2右外连接 RIGHT OUTER JOIN

==语法==：

```mysql
FROM A表 RIGHT JOIN B表
ON 关联条件
WHERE 等其他子句;
```

例子：

```mysql
SELECT e.last_name, e.department_id, d.department_name
FROM employees e
RIGHT OUTER JOIN departments d
ON (e.department_id = d.department_id) ;
# 结果集中将包含右表（departments）不满足匹配条件的数据
```

==注意==：一般使用右连接时，省略`OUTER`，只写`RIGHT JOIN`即表示内连接

#### 4.3.3满外连接 FULL OUTER JOIN

* 满外连接的结果 = 左右表匹配的数据 + 左表没有匹配到的数据 + 右表没有匹配到的数据。
* SQL99是支持满外连接的。使用FULL JOIN 或 FULL OUTER JOIN来实现。
* ==需要注意的是==，==MySQL不支持FULL JOIN==，但是可以用 LEFT JOIN `UNION` RIGHT JOIN代替。

### 4.4UNION的使用

==合并查询结果== 利用UNION关键字，可以给出多条SELECT语句，并将它们的结果组合成单个结果集。合并时，==两个表对应的列数和数据类型必须相同，并且相互对应==。各个SELECT语句之间使用`UNION`或`UNIONALL`关键字分隔。

```mysql
SELECT column,... FROM table1
UNION [ALL]
SELECT column,... FROM table2
```

#### UNION    vs    UNIONALL

==UNION==

UNION 操作符返回两个查询的结果集的并集，去除重复记录。

![image-20220924125420455](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220924125420455.png)

==UNIONALL==

UNION ALL操作符返回两个查询的结果集的并集。对于两个结果集的重复部分，==不去重==。

![image-20220924125819001](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220924125819001.png)

==注意==

执行UNION ALL语句时所需要的资源比UNION语句少。如果明确知道合并数据后的结果数据
不存在重复数据，或者不需要去除重复的数据，则尽量使用`UNION ALL`语句，以提高数据查询的效
率。

举例：查询部门编号>90或邮箱包含a的员工信息

```mysql
#方式1
SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;
```

```mysql
#方式2
SELECT * FROM employees WHERE email LIKE '%a%'
UNION
SELECT * FROM employees WHERE department_id>90;
```

举例：查询中国用户中男性的信息以及美国用户中年男性的用户信息

```mysql
SELECT id,cname FROM t_chinamale WHERE csex='男'
UNION ALL
SELECT id,tname FROM t_usmale WHERE tGender='male';
```

## 5. 7种JOIN的实现



![image-20220924130439978](https://raw.githubusercontent.com/Eneru7/img/main/img_folder/image-20220924130439978.png)

### 5.1代码实现

```mysql
#中图：内连接 A∩B
SELECT employee_id,last_name,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```mysql
#左上图：左外连接
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```mysql
#右上图：右外连接
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```mysql
#左中图：A - A∩B
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
```

```mysql
#右中图：B-A∩B
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL
```

```mysql
#左下图 = 左上图 + 右中图  或者 （左下图 = 左中图 + 右上图）
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL #没有去重操作，效率高
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;
```

```mysql
#右下图 = 左中图 + 右中图
# A ∪ B- A ∩ B 或者 (A - A ∩ B) ∪ （B - A ∩ B）
SELECT employee_id,last_name,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,last_name,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL
```

