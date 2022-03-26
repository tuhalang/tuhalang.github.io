---
layout: post
title: Kết nối oracle database sử dụng Dbeaver
categories: [Oracle, Database]
description: Kết nối oracle database sử dụng Dbeaver
keywords: Database, Oracle
---

I. Download && Install Dbeaver
- Các bạn có thể tải Dbeaver từ trang chủ [Download Dbeaver](https://dbeaver.io/download/)

II. Connect database

- Download ojdbc tại [đây](https://www.oracle.com/database/technologies/jdbcdriver-ucp-downloads.html)

- Chọn New Database Connection -> Chọn Oracle -> bấm Next sau đó nhập các thông tin của database vào:

![anh](https://tuhalang.github.io/assets/images/ora.png)

- Tiếp theo chọn Edit Driver Setting -> chọn Add File chỏ đến file ojdbc đã tải ở trên sau đó bấm Ok 
- Chọn Test Connection để kiểm tra kết nối sau đó bấm Finish để kết thúc.

III. Tạo user mới

- Câu lệnh dưới đây sẽ thực hiện tạo mới user và thực hiện gán quyền admin cho nó.

```sql
-- create new user
CREATE USER <<USER_NAME>> IDENTIFIED BY <<PASSWORD>>;

-- grant priviledges
GRANT CONNECT, RESOURCE, DBA TO <<USER_NAME>>;
```

- Ở đây mình sẽ tạo user với tên mình: 

```sql
-- create new user
CREATE USER PHAMHUNG IDENTIFIED BY 123456;

-- grant priviledges
GRANT CONNECT, RESOURCE, DBA TO PHAMHUNG;
```

IV. Import sample database

- Các bạn đăng nhập lại cơ sở dữ liệu bằng tài khoản vừa tạo.
- Tải sample database [tại đây](https://tuhalang.github.io/assets/sample_database/table.sql) và [tại đây](https://tuhalang.github.io/assets/sample_database/data.sql)
- Import lần lượt các file theo thứ tự table.sql -> data.sql 
- Để import file ta thực hiện câu lệnh sau:

```
SQL>@path_to_sql_file
```

- Ví dụ:

```sql
@/home/tuhalang/Downloads/table.sql
@/home/tuhalang/Downloads/data.sql
```

- Và đây là kết quả sau khi import:

![image](https://tuhalang.github.io/assets/images/table.png)
