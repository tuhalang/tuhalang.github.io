---
layout: post
title: Oracle Order By
categories: [Oracle, Database]
description: Oracle Order By
keywords: Database, Oracle
---

**Tóm tắt**: Trong bài viết này chúng ta sẽ học các sử dụng *ORDER BY* để sắp xếp dữ liệu theo một hay nhiều cột theo thứ tự tăng dần hoặc giảm dần.

### 1. Tóm tắt lý thuyết

Trong Oracle, một bảng khi lưu trữ thì các bản ghi của nó không được sắp sếp khi insert vào bảng. Vì vậy khi muốn lấy dữ liệu theo thứ tự nào đó thì ta phải chỉ định trong câu truy vấn.

Chúng ta có thể sắp xếp được các dữ liệu vè số, chữ, thời gian, ...

Ví dụ, chúng ta có thể muốn lấy tất cả khách hàng có tên được sắp xếp theo bảng chữ cái hoặc hạn mức tín dụng theo thứ tự tù thấp đến cao.

Để sắp xếp dữ liệu, chúng ta cần thêm ORDER BY vào câu lệnh SELECT như sau:

```sql
SELECT 
    column_1,
    column_2,
    ... 
FROM 
    table_name 
ORDER BY 
    column_1 [ASC | DESC] [NULLS FIRST | NULLS LAST],
    column_2 [ASC | DESC] [NULLS FIRST | NULLS LAST],
    ...
```

Note:
+ ASC : sắp xếp tăng dần 
+ DESC : sắp xếp giảm dẫn 
+ NULLS FIRST : đặt giá trị null trước 
+ NULLS LAST : đặt giá trị null sau

### 2. Ví dụ
Chúng ta sẽ sử dụng bảng **customers** để thực hiện các ví dụ.

Đầu tiên chúng ta sẽ lấy ra *name*, *address*, *credit_limit*:

```sql
SELECT name, address, credit_limit FROM customers;
```

![anh](https://tuhalang.github.io/assets/images/unoder.png)

Như chúng ta có thể thấy dữ liệu không được sắp xếp theo bất kì thứ tự nào.

**Sắp xếp theo một cột:** 
- Sắp xếp dữ liệu name tăng dần theo bảng chữ cái.

```sql
SELECT name, address, credit_limit FROM customers ORDER BY name ASC;
```

Hoặc

```sql
SELECT name, address, credit_limit FROM customers ORDER BY name;
```

Khi không chỉ định ASC hay DESC thì mặc định sẽ là ASC

![anh](https://tuhalang.github.io/assets/images/sorted_name_asc.png)

- Sắp xếp dữ liệu name giảm dần theo bảng chữ cái.

```sql
SELECT name, address, credit_limit FROM customers ORDER BY name DESC;
```

![anh](https://tuhalang.github.io/assets/images/sorted_name_desc.png)

**Sắp xếp theo nhiều cột:**

- Sắp xếp *contacts* theo *first_name* tăng dần và *last_name* giảm dần.

```sql
SELECT first_name, last_name FROM contacts ORDER BY first_name, last_name DESC;
```

![anh](https://tuhalang.github.io/assets/images/sorted_mul.png)

Đầu tiên *first_name* sẽ được sắp xếp trước theo thứ tự tăng dần sau đó *last_name* mới được sắp xếp theo thứ tự giảm dần. 

**Sắp xếp theo vị trí cột:**

Bạn không cần chỉ định tên của cột để sắp xếp dữ liệu mà chỉ cần chỉ định sắp xếp theo vị trí của cột 

```sql
SELECT 
    name,
    credit_limit
FROM 
    customers
ORDER BY
    2 DESC,
    1
```

Câu truy vấn này sẽ tương đương với:

```sql
SELECT 
    name,
    credit_limit
FROM 
    customers
ORDER BY
    credit_limit DESC,
    name
```

**Sắp xếp với giá trị NULL:**

Chúng ta hãy xem bảng **locations**

|LOCATIONS  |
|-----------|
|LOCATION_ID|
|ADDRESS|
|POSTAL_CODE|
|CITY|
|STATE|
|COUNTRY_ID|

Câu truy vấn dưới đây sẽ lấy thông tin **location** và sắp xếp theo *city* và *state*

```sql
SELECT 
    country_id,
    city,
    state
FROM 
    locations
ORDER BY
    state
```
Ta sẽ được kết quả:

![anh](https://tuhalang.github.io/assets/images/sorted_null_default.png)

Tuy nhiên ta có thể chỉ đinh giá trị null hiện thị ở đầu hoặc cuối bằng cách sử dụng thêm *NULLS FIRST* hoặc *NULLS LAST*

```sql
SELECT 
    country_id,
    city,
    state
FROM 
    locations
ORDER BY
    state NULLS FIRST
```

Khi đó ta sẽ được kết quả:

![anh](https://tuhalang.github.io/assets/images/sorted_nulls_first.png)

**Sắp xếp theo hàm hoặc biểu thức:**

```sql
SELECT 
    customer_id,
    name
FROM
    customers
ORDER BY 
    UPPER(name)
```

![anh](https://tuhalang.github.io/assets/images/sorted_func.png)


**Sắp xếp theo thời gian**

```sql
SELECT 
   order_id, 
   customer_id, 
   status, 
   order_date
FROM 
   orders
ORDER BY 
   order_date DESC;
```

![anh](https://tuhalang.github.io/assets/images/sorted_time.png)