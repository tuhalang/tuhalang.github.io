---
layout: post
title: Oracle Dual Table
categories: [Oracle, Database]
description: Oracle Dual Table
keywords: Database, Oracle
---

Trong oracle DUAL table là một bảng đặc biệt được sử dụng để tính toán các biểu thức hoặc để gọi các hàm có sẵn.

Như trong bài trước mình đã nói trong câu lệnh SELECT bắt buộc phải có FROM, tuy nhiên có một số câu truy vấn không yêu cầu bất kì bảng nào. 

VD: 

```sql
SELECT UPPER('This is a string') FROM what_the_hell_table;
```
May mắn là Oracle đã cung cấp cho chúng ta một bảng DUAL thuộc schema của user SYS nhưng có thể được truy cập bởi tất cả các user.

Bảng DUAL chỉ có một cột có tên là DUMMY với kiểu dữ liệu varchar2() và chỉ có một dòng dữ liệu cứa một giá trị 'X'

Bạn có thể thử truy vấn bằng các thực thi câu lệnh:

```sql
SELECT * FROM DUAL;
```

Khi đó ta có thể viết lại câu truy vấn bên trên thành:

```sql
SELECT UPPER('This is a string') FROM dual;
```

Chúng ta cũng có thể sử dụng tương tự cho việc tính toán các biểu thức toán học:

```sql
SELECT (10+20)*15 FROM dual;
```