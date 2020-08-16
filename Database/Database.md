# Why database

![image-20200716220926516](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200716220926516.png)

# Scaling

![image-20200716223546267](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200716223546267.png)

# SQL vs NoSQL

https://academind.com/learn/web-dev/sql-vs-nosql/

![image-20200716223824491](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200716223824491.png)

|                                             | SQL Databases                                                | NoSQL Databases                                              |
| ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Types**                                   | One type (SQL database) with minor variations                | Many different types including key-value stores,[document databases](https://www.mongodb.com/document-databases), wide-column stores, and graph databases |
| **Examples**                                | MySQL, Postgres, Microsoft SQL Server, Oracle Database       | MongoDB, Cassandra, HBase, Neo4j                             |
| **Data Storage Model**                      | much like a **spreadsheet**.<br />Data store in **tables** (Users, Orders, Products) and can have **relationships**  with other tables<br />A table have **fields** (**columns**) to define the data (User have ID, email, name)<br />Data is called **record** (**rows**) | Tables are Collections<br />Record (rows) are Documents store in a format (JSON,XML,...)<br />User **duplicated** and **nested** data instead of relation |
| **Schemas**                                 | Structure and data types are **fixed** in advance.           | Typically **dynamic** (**schemaless**), with some enforcing data validation rules. Applications can add new fields on the fly, and unlike SQL table rows, dissimilar data can be stored together as necessary. |
| **Scaling**                                 | Vertically, meaning a single server must be made increasingly powerful in order to deal with increased demand. It is possible to spread SQL databases over many servers, but significant additional engineering is generally required, and core relational features such as JOINs, referential integrity and transactions are typically lost. | Horizontally + Vertically, meaning that to add capacity, a database administrator can simply add more commodity servers or cloud instances. The database automatically spreads data across servers as necessary. |
| **Supports multi-record ACID transactions** | Yes                                                          | Mostly no. MongoDB 4.0 and beyond support multi-document ACID transactions.[Learn more](https://www.mongodb.com/transactions) |
| **Data Manipulation**                       | Specific language using Select, Insert, and Update statements, e.g. SELECT fields FROM table WHERE… | Through object-oriented APIs                                 |
| **Consistency**                             | Can be configured for strong consistency                     | Depends on product. Some provide strong consistency (e.g., MongoDB, with tunable consistency for reads) whereas others offer eventual consistency (e.g., Cassandra). |

# SQL

![image-20200716221148542](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200716221148542.png)

## ACID 

![img](https://tedu.com.vn/UploadData/images/acid-600x338.png)

**Atomicity** là một đề xuất tất cả hoặc không có gì. Tính chất này đảm bảo rằng khi một giao dịch liên quan đến hai hay nhiều xử lý, hoặc là tất cả các xử lý được thực hiện hoặc không có xử lý được thực hiện.
**Consistency** đảm bảo rằng một giao dịch không bao giờ được thông qua cơ sở dữ liệu của bạn trong tình trạng dở dang. Tính chất này, hoặc là tạo ra toàn bộ trạng thái mới hoặc rollback tất cả các xử lý để về trạng thái ban đầu, nhưng không bao giờ thông qua cơ sở dữ liệu trong trạng thái dở dang.
**Isolation** giữ giao dịch tách rời nhau cho đến khi chúng đã hoàn tất. Tính chất này đảm bảo rằng hai hoặc nhiều giao dịch không bao giờ được trộn lẫn với nhau, tạo ra dữ liệu không chính xác và không phù hợp.
**Durability** đảm bảo rằng cơ sở dữ liệu sẽ theo dõi các thay đổi cấp phát trong một cách mà các máy chủ có thể phục hồi từ một sự kết thúc bất thường. Tính chất này đảm bảo rằng trong trường hợp thất bại hay dịch vụ khởi động lại các dữ liệu có sẵn trong  trước khi gặp lỗi.

## Transaction 

Transaction can be defined as a collection of task that are considered as minimum processing unit. Each minimum processing unit can not be divided further.

The main operation of a transaction are read and write.

All transaction must contain four properties that commonly known as ACID properties for the purpose of ensuring accuracy , completeness and data integrity.

| Read phenomena\ Isolation level | Dirty reads | Lost updates | Non-repeatable reads |  Phantoms   |
| :-----------------------------: | :---------: | :----------: | :------------------: | :---------: |
|        Read Uncommitted         |  may occur  |  may occur   |      may occur       |  may occur  |
|         Read Committed          | don't occur |  may occur   |      may occur       |  may occur  |
|         Repeatable Read         | don't occur | don't occur  |     don't occur      |  may occur  |
|          Serializable           | don't occur | don't occur  |     don't occur      | don't occur |

## Lost updates

A **lost update** occurs when two different transactions are trying to **update** the same column on the same row within a database at the same time.

![Kết quả hình ảnh cho Lost updates](https://www.researchgate.net/profile/Shamim_Abdul_Nazar/publication/299424245/figure/fig1/AS:462101773852674@1487185056862/Example-for-Lost-Update-5.png)

## Dirty reads

A *dirty read* (aka *uncommitted dependency*) occurs when a transaction is allowed to read data from a row that has been modified by another running transaction and not yet committed.

| Transaction 1                                                | Transaction 2                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `/* Query 1 */ SELECT age FROM users WHERE id = 1; /* will read 20 */ ` |                                                              |
|                                                              | `/* Query 2 */ UPDATE users SET age = 21 WHERE id = 1; /* No commit here */ ` |
| `/* Query 1 */ SELECT age FROM users WHERE id = 1; /* will read 21 */ `  (This is dirty read here) |                                                              |
|                                                              | `ROLLBACK; `                                                 |

## Non-repeatable reads

A *non-repeatable read* occurs, when during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

| Transaction 1                                                | Transaction 2                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `/* Query 1 */ SELECT * FROM users WHERE id = 1; `           |                                                              |
|                                                              | `/* Query 2 */ UPDATE users SET age = 21 WHERE id = 1; COMMIT; /* in multiversion concurrency   control, or lock-based READ COMMITTED */ ` |
| `/* Query 1 */ SELECT * FROM users WHERE id = 1; /* lock-based REPEATABLE READ */` |                                                              |

## Phantom reads

A *phantom read* occurs when, in the course of a transaction, new rows are added or removed by another transaction to the records being read.

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Transaction 1                                                | Transaction 2                                                |
| `/* Query 1 */ SELECT * FROM users WHERE age BETWEEN 10 AND 30; ` |                                                              |
|                                                              | `/* Query 2 */ INSERT INTO users(id,name,age) VALUES ( 3, 'Bob', 27 ); COMMIT; ` |
| `/* Query 1 */ SELECT * FROM users WHERE age BETWEEN 10 AND 30; COMMIT; ` |                                                              |

# NoSQL

![image-20200716223418804](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200716223418804.png)

# N+1 problem

- 1 query to get user `"1"`
- N query to get a data relative to user `"N"

=>  select in () || joins

