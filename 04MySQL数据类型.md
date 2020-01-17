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



