# SQL vs noSQL

|                                             | SQL Databases                                                | NoSQL Databases                                              |
| ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Types**                                   | One type (SQL database) with minor variations                | Many different types including key-value stores,[document databases](https://www.mongodb.com/document-databases), wide-column stores, and graph databases |
| **Development History**                     | Developed in 1970s to deal with first wave of data storage applications | Developed in late 2000s to deal with limitations of SQL databases, especially scalability, multi-structured data, geo-distribution and agile development sprints |
| **Examples**                                | MySQL, Postgres, Microsoft SQL Server, Oracle Database       | MongoDB, Cassandra, HBase, Neo4j                             |
| **Data Storage Model**                      | Individual records (e.g., 'employees') are stored as rows in tables, with each column storing a specific piece of data about that record (e.g., 'manager,' 'date hired,' etc.), much like a spreadsheet. Related data is stored in separate tables, and then joined together when more complex queries are executed. For example, 'offices' might be stored in one table, and 'employees' in another. When a user wants to find the work address of an employee, the database engine joins the 'employee' and 'office' tables together to get all the information necessary. | Varies based on database type. For example, key-value stores function similarly to SQL databases, but have only two columns ('key' and 'value'), with more complex information sometimes stored as BLOBs within the 'value' columns. Document databases do away with the table-and-row model altogether, storing all relevant data together in single 'document' in JSON, XML, or another format, which can nest values hierarchically. |
| **Schemas**                                 | Structure and data types are fixed in advance. To store information about a new data item, the entire database must be altered, during which time the database must be taken offline. | Typically dynamic, with some enforcing data validation rules. Applications can add new fields on the fly, and unlike SQL table rows, dissimilar data can be stored together as necessary. For some databases (e.g., wide-column stores), it is somewhat more challenging to add new fields dynamically. |
| **Scaling**                                 | Vertically, meaning a single server must be made increasingly powerful in order to deal with increased demand. It is possible to spread SQL databases over many servers, but significant additional engineering is generally required, and core relational features such as JOINs, referential integrity and transactions are typically lost. | Horizontally, meaning that to add capacity, a database administrator can simply add more commodity servers or cloud instances. The database automatically spreads data across servers as necessary. |
| **Development Model**                       | Mix of open technologies (e.g., Postgres, MySQL) and closed source (e.g., Oracle Database) | Open technologies                                            |
| **Supports multi-record ACID transactions** | Yes                                                          | Mostly no. MongoDB 4.0 and beyond support multi-document ACID transactions.[Learn more](https://www.mongodb.com/transactions) |
| **Data Manipulation**                       | Specific language using Select, Insert, and Update statements, e.g. SELECT fields FROM table WHERE… | Through object-oriented APIs                                 |
| **Consistency**                             | Can be configured for strong consistency                     | Depends on product. Some provide strong consistency (e.g., MongoDB, with tunable consistency for reads) whereas others offer eventual consistency (e.g., Cassandra). |

# **ACID** 

![img](https://tedu.com.vn/UploadData/images/acid-600x338.png)

**Atomicity** là một đề xuất tất cả hoặc không có gì. Tính chất này đảm bảo rằng khi một giao dịch liên quan đến hai hay nhiều xử lý, hoặc là tất cả các xử lý được thực hiện hoặc không có xử lý được thực hiện.
**Consistency** đảm bảo rằng một giao dịch không bao giờ được thông qua cơ sở dữ liệu của bạn trong tình trạng dở dang. Tính chất này, hoặc là tạo ra toàn bộ trạng thái mới hoặc rollback tất cả các xử lý để về trạng thái ban đầu, nhưng không bao giờ thông qua cơ sở dữ liệu trong trạng thái dở dang.
**Isolation** giữ giao dịch tách rời nhau cho đến khi chúng đã hoàn tất. Tính chất này đảm bảo rằng hai hoặc nhiều giao dịch không bao giờ được trộn lẫn với nhau, tạo ra dữ liệu không chính xác và không phù hợp.
**Durability** đảm bảo rằng cơ sở dữ liệu sẽ theo dõi các thay đổi cấp phát trong một cách mà các máy chủ có thể phục hồi từ một sự kết thúc bất thường. Tính chất này đảm bảo rằng trong trường hợp thất bại hay dịch vụ khởi động lại các dữ liệu có sẵn trong  trước khi gặp lỗi.

# Transaction 

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
| `/* Query 1 */ SELECT age FROM users WHERE id = 1; /* will read 21 */ ` |                                                              |
|                                                              | `ROLLBACK; /* lock-based DIRTY READ */`                      |

## Non-repeatable reads

A *non-repeatable read* occurs, when during the course of a transaction, a row is retrieved twice and the values within the row differ between reads.

| Transaction 1                                                | Transaction 2                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `/* Query 1 */ SELECT * FROM users WHERE id = 1; `           |                                                              |
|                                                              | `/* Query 2 */ UPDATE users SET age = 21 WHERE id = 1; COMMIT; /* in multiversion concurrency   control, or lock-based READ COMMITTED */ ` |
| `/* Query 1 */ SELECT * FROM users WHERE id = 1; COMMIT; /* lock-based REPEATABLE READ */` |                                                              |

## Phantom reads

A *phantom read* occurs when, in the course of a transaction, new rows are added or removed by another transaction to the records being read.

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Transaction 1                                                | Transaction 2                                                |
| `/* Query 1 */ SELECT * FROM users WHERE age BETWEEN 10 AND 30; ` |                                                              |
|                                                              | `/* Query 2 */ INSERT INTO users(id,name,age) VALUES ( 3, 'Bob', 27 ); COMMIT; ` |
| `/* Query 1 */ SELECT * FROM users WHERE age BETWEEN 10 AND 30; COMMIT; ` |                                                              |

# N+1 problem

Giả sử ta có một cơ sở dữ liệu, trong đó table `post` có khóa ngoại `user_id`

Thực hiện truy vấn vào cơ sở dữ liệu và lấy `tất cả` User `kèm theo` các Post của User đó:

- Một truy vấn để lấy ra tất cả users => đây chính là 1 trong `"N+1"`
- Ba truy vấn để lấy ra các post tương ứng với các user trong cơ sở dữ liệu => đây chính là `N` trong `"N+1"`

Đối với những hệ thống có số lượng `bản ghi` lớn (cỡ như phải trả về 1000 user thì chúng ta phải thực hiện 1001 truy vấn) 

=>  select in () || joins