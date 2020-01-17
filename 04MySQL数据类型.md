# 04MySQL数据类型

# 1.数据类型分类

- 数值类型
  - bit(M):位类型.M指定位数，默认值为1，范围1-64
  - tinyint:有符号范围-128-127，无符号范围0-255，默认有符号
  - int:有符号范围-2^31~2^31-1,无符号范围0~2^32-1
  - float[(M,D)]:M指定显示长度，D指定小数位数，占用4字节
- 文本、二进制类型
  - char():固定长度字符串，最大255
  - varchar():可变长度字符串，最大长度65535
  - text:大文本，不支持全文索引和默认值
- 时间日期
  - date/datetime/timestamp:日期类型
- String类型
  - enum:字符串对象，其值来自表创建时在列规定中显示枚举的一列值
  - set：字符串对象，可以有0或多个值

# 2.数值类型

## 2.1bit类型

语法：

bit[(M)]:位字段类型。M表示每个值的位数，范围1-64。默认1

栗子：

1. 创建一个表tt1;

   ```mysql
   mysql> create table tt1(id bit(8));
   Query OK, 0 rows affected (0.27 sec)
   ```

2. 插入一系列数据；

   ```mysql
   mysql> insert into tt1 values(10);
   Query OK, 1 row affected (0.03 sec)
   
   mysql> insert into tt1 values(20);
   Query OK, 1 row affected (0.01 sec)
   
   mysql> insert into tt1 values(50);
   Query OK, 1 row affected (0.01 sec)
   
   mysql> insert into tt1 values(65);
   Query OK, 1 row affected (0.01 sec)
   ```

3. 查看表中数据；

   ```mysql
   mysql> select * from tt1;
   +------+
   | id   |
   +------+
   |
       |
   |     |
   | 2    |
   | A    |
   +------+
   4 rows in set (0.00 sec)
   ```

4. 发现规律；

   **bit字段在显示时，是按照ASCII码对应的值显示；**

   如果在一个表中只需要存储1和0两种数据时，可以定义bit(1)，这样可以节省不少空间；

## 2.2tinyint类型

语法：

tinyint[unsigned]:有符号范围-128~127;无符号范围0~255;默认有符号；

栗子：数值越界测试

1. 创建一个表tt2;

   ```mysql
   mysql> create table tt2(num tinyint);
   Query OK, 0 rows affected (0.10 sec)
   ```

2. 插入越界数据；

   ```mysql
   mysql> insert into tt2 values(128);
   ERROR 1264 (22003): Out of range value for column 'num' at row 1
   ```

3. 发现规律；

   在MySQL中，整型可以指定为有符号/无符号，默认是有符号的；

4. 使用unsigned声明字段无符号

   ```mysql
   mysql> create table tt3(num tinyint unsigned);
   Query OK, 0 rows affected (0.10 sec)
   
   mysql> insert into tt3 values(-1);
   ERROR 1264 (22003): Out of range value for column 'num' at row 1
   mysql> insert into tt3 values(255);
   Query OK, 1 row affected (0.01 sec)
   
   mysql> select * from tt3;
   +------+
   | num  |
   +------+
   |  255 |
   +------+
   1 row in set (0.00 sec)
   ```

int类型和tinyint类型唯一的区别在于存储数值大小不同；

## 2.3float类型

语法：

float[(m,d)]:m指定显示长度，d指定小数位数，占用空间4个字节

栗子：

1. 创建一个表tt4;

   ```mysql
   mysql> create table tt4(salary float(4,2));
   Query OK, 0 rows affected (0.10 sec)
   ```

2. 插入数据；

   ```mysql
   mysql> insert into tt4 values(99.99);
   Query OK, 1 row affected (0.02 sec)
   
   mysql> insert into tt4 values(98.987);
   Query OK, 1 row affected (0.01 sec)
   ```

3. 显示表中数据；

   ```mysql
   mysql> select * from tt4;
   +--------+
   | salary |
   +--------+
   |  99.99 |
   |  98.99 |
   +--------+
   2 rows in set (0.00 sec)
   ```

4. 发现规律；

   在MySQL中，插入的数值会根据表中的规定进行存储显示；

# 3.字符串类型

## 3.1char类型

语法：

char(L):固定长度字符串，L是可以存储的长度，单位为字符，最大长度值为255

栗子：

1. 创建一个表tt5;

   ```mysql
   mysql> create table tt5(name char(2));
   Query OK, 0 rows affected (0.19 sec)
   ```

2. 插入数据；

   ```mysql
   mysql> insert into tt5 values('ab');
   Query OK, 1 row affected (0.01 sec)
   
   mysql> insert into tt5 values('中国');
   Query OK, 1 row affected (0.01 sec)
   ```

3. 显示表中的数值；

   ```mysql
   mysql> select * from tt5;
   +------+
   | name |
   +------+
   | ab   |
   | 中国 |
   +------+
   2 rows in set (0.00 sec)
   ```

4. 发现规律；

   char(2)表示可以存放两个字符，可以是字母或汉子，但是不能超过2个，最多只能是255；

   ```mysql
   mysql> create table tt5(name char(256));
   ERROR 1074 (42000): Column length too big for column 'name' (max = 255); use BLOB or TEXT instead
   ```

## 3.2varchar类型

语法：

varchar(L):可变长度字符串，L表示字符长度，最大长度65535个字节；

栗子：

1. 创建一个表tt6;

   ```mysql
   mysql> create table tt6(name varchar(6));
   Query OK, 0 rows affected (0.09 sec)
   ```

2. 插入数据；

   ```mysql
   mysql> insert into tt6 values('hello');
   Query OK, 1 row affected (0.01 sec)
   
   mysql> insert into tt6 values('我爱你，中国');
   Query OK, 1 row affected (0.01 sec)
   ```

3. 显示表中的数据；

   ```mysql
   mysql> select * from tt6;
   +--------------+
   | name         |
   +--------------+
   | hello        |
   | 我爱你，中国 |
   +--------------+
   2 rows in set (0.00 sec)
   ```

4. 发现规律；

   varchar长度可以指定为0到65535之间的值，但是有1-3个字节用于记录数据大小，所以说有效字节数为65532.

## 3.3char和varchar比较

如何选择定长或变长字符串？

- 如果数据确定长度都一样，就使用定长(char),比如：身份证，手机号，Md5
- 如果数据长度有变化，就使用变长(varchar),比如：名字，地址
- 定长的磁盘空间比较浪费，但是效率高；
- 变长的磁盘空间比较节省，但是效率低；

# 4.日期和时间类型

常见的日期有以下三种：

- datetime:时间日期格式'yyyy-mm-dd HH:ii:ss'表示范围从1000到9999，占用八字节
- date:日期'yyyy-mm-dd',占用三字节
- timestamp:时间戳

栗子：

1. 创建一个表tt7;

   ```mysql
   mysql> create table tt7(t1 date,t2 datetime,t3 timestamp);
   Query OK, 0 rows affected (0.15 sec)
   ```

2. 插入数据；

   ```mysql
   mysql> insert into tt7(t1,t2) values('1997-7-1','2008-8-8 12:1:1');
   Query OK, 1 row affected (0.02 sec)
   ```

3. 更新数据；

   ```mysql
   mysql> update tt7 set t1='2000-6-1';
   Query OK, 1 row affected (0.01 sec)
   Rows matched: 1  Changed: 1  Warnings: 0
   ```

4. 显示表中数据；

   更新前数据：

   ```mysql
   mysql> select * from tt7;
   +------------+---------------------+------+
   | t1         | t2                  | t3   |
   +------------+---------------------+------+
   | 1997-07-01 | 2008-08-08 12:01:01 | NULL |
   +------------+---------------------+------+
   1 row in set (0.00 sec)
   ```

   更新后数据：

   ```mysql
   mysql> select * from tt7;
   +------------+---------------------+------+
   | t1         | t2                  | t3   |
   +------------+---------------------+------+
   | 2000-06-01 | 2008-08-08 12:01:01 | NULL |
   +------------+---------------------+------+
   1 row in set (0.00 sec)
   ```

