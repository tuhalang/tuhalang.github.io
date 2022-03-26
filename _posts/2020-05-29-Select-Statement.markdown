---
layout: post
title: Oracle Select Statement
categories: [Oracle, Database]
description: Oracle Select Statement
keywords: Database, Oracle
---

I. Tóm tắt lý thuyết

Trong Oracle hay bất kì RDBMS nào thì một bảng bao gồm các cột và hàng.

Như trong bảng **customers** trong sample database có các cột sau: *customer_id*, *name*, *address*, *website* và *credit_limit*.

|CUSTOMERS |
|----------|
|CUSTOMER_ID|
|NAME|
|ADDRESS|
|WEBSITE|
|CREDIT_LIMIT|

Trong bảng **customers** không chỉ có các cột mà trong các cột còn chứa các dữ liệu của nó nữa.

Khi đó để lấy dữ liệu từ một hay nhiều cột trong bảng ta sẽ dùng câu lệnh **SELECT**

Cú pháp của câu lệnh **SELECT** sẽ như sau:

```sql
SELECT column_1, column_2, ... FROM table_name;
```
Chú ý: Không giống như các RDBMS khác có thể không cần FROM table_name trong câu SELECT, Oracle bắt buộc phải có FROM t

II. Ví dụ 

1. Truy vấn dữ liệu từ một cột 

```sql
SELECT name FROM customers;
```

Sau khi thực thi câu truy vấn sẽ thu được kết quả:

![anh](https://tuhalang.github.io/assets/images/select-one.png)

2. Truy vấn dữ liệu từ nhiều cột 

```sql
SELECT name, address FROM customers;
```

Sau khi thực thi câu truy vấn sẽ thu được kết quả:

![anh](https://tuhalang.github.io/assets/images/select-mul.png)

3. Truy vấn tất cả các cột

```sql
SELECT customer_id, name, address, website, credit_limit FROM customers;
```

Hoặc 

```sql
SELECT * FROM customers;
```

Thực thi hai câu lệnh trên sẽ đều thu đưọc kết quả giống nhau:

![anh](https://tuhalang.github.io/assets/images/select-all.png)
