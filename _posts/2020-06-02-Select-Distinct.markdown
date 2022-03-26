---
layout: post
title: Select distinct
categories: [Oracle, Database]
description: Select distinct
keywords: Database, Oracle
---

**Tóm tắt:** Trong bài viết này chúng ta sẽ cùng tìm hiểu về cách để truy vấn lấy ra dữ liệu không bị trùng lặp sử dụng ***SELECT DISTINCT***

#### 1. Tóm tắt lý thuyết

- DISTINCT được sử dụng trong câu lệnh SELECT dùng để lọc các giá trị trùng nhau trong kết quả truy vấn. Điều này đảm bảo rằng các giá trị trả về là duy nhất
- Cú pháp của SELECT DISTINCT sẽ như sau:

```sql
SELECT DISTINCT column_1 FROM table_name;
```

- Trong câu truy vấn này dữ liệu trong cột column_1 sẽ được so sánh để lọc bỏ giá trị trùng.

- Bạn cũng có thể SELECT DISTINCT trên nhiều cột:

```sql
SELECT DISTINCT 
    column_1,
    column_2,
    column_3
FROM table_name;
```

- Khi đó dữ liệu trong cả ba cột *column_1*, *column_2*, *column_3* sẽ được kết hợp và so sánh để lọc bỏ giá trị trùng. Điều đó có nghĩa là 2 bản ghi được xem là trùng lặp khi và chỉ khi cả 3 cột *column_1*, *column_2* và *column_3* có giá trị giống nhau.

#### 2. Ví dụ 

##### a) SELECT DISTINCT trên một cột

- Chúng ta hãy xem xét bảng **contacts**

|CONTACTS|
|--------|
|CONTACT_ID|
|FIRST_NAME|
|LAST_NAME|
|EMAIL|
|PHONE|
|CUSTOMER_ID|

- Câu truy vấn sau sẽ lấy ra tất cả các first_name 

```sql
SELECT
    first_name
FROM
    contacts
ORDER BY
    first_name;
```

- Khi đó sẽ được kết quả 319 bản ghi

![anh](https://tuhalang.github.io/assets/images/select_non_distinct.png)

- Còn khi thực hiện SELECT DISTINCT

```sql
SELECT DISTINCT
    first_name
FROM
    contacts
ORDER BY
    first_name;
```

- Sẽ được kết quả sau với 302 bản ghi có nghĩa là đã có 17 bản ghi bị trùng lặp đã bị loại bỏ

![anh](https://tuhalang.github.io/assets/images/select_distinct.png)

##### a) SELECT DISTINCT trên nhiều cột

- Chúng ta hãy xem xét bảng **order_items**

|ORDER_ITEMS|
|--------|
|ITEM_ID|
|PRODUCT_ID|
|QUANTITY|
|UNIT_PRICE|

- Chúng ta sẽ lấy giá trị của *product_id* và *quantity*

- Truy vấn không distinct:

```sql
SELECT
    product_id,
    quantity
FROM 
    ORDER_ITEMS
ORDER BY 
    product_id;
```
- Kết quả sẽ được 665 bản ghi:

![anh](https://tuhalang.github.io/assets/images/select_two_col_non_distinct.png)

- Truy vấn sử dụng distinct:

```sql
SELECT DISTINCT
    product_id,
    quantity
FROM
    order_items
ORDER BY
    product_id;
```

- Kết quả sẽ được 657 bản ghi:

![anh](https://tuhalang.github.io/assets/images/select_two_col_distinct.png)

##### c) SELECT DISTINCT với giá trị NULL 

- Nếu sử dụng SELECT DISTINCT để truy vấn từ cột mà có nhiều giá trị NULL thì kết quả sẽ chỉ bao gồm một giá trị NULL duy nhất.

- Chúng ta hãy xem bảng **locations**:

|LOCATIONS|
|---------|
|LOCATION_ID|
|ADDRESS|
|POSTAL_CODE|
|CITY|
|STATE|
|COUNTRY_ID|

- Truy vấn lấy state không distinct

```sql
SELECT 
    state 
FROM 
    locations 
ORDER BY 
    state NULLS FIRST;
```

- Ta sẽ được kết quả:

![anh](https://tuhalang.github.io/assets/images/select_null_non_distinct.png)

- Truy vấn lấy state sử dụng distinct

```sql
SELECT DISTINCT
    state 
FROM 
    locations 
ORDER BY 
    state NULLS FIRST;
```

- Ta sẽ được kết quả:

![anh](https://tuhalang.github.io/assets/images/select_null_distinct.png)
