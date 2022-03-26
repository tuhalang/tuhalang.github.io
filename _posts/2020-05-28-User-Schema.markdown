---
layout: post
title: So sánh User và Schema trong Oracle
categories: [Oracle, Database]
description: So sánh User và Schema trong Oracle
keywords: Database, Oracle
---

Như ở bài trước khi chúng ta tạo một user thì trong database sẽ xuất hiện một schema có tên giống hệt user chúng ta vừa tạo.

![anh](https://tuhalang.github.io/assets/images/schema.png)

Vậy user và schema khác nhau như thế nào ?

- Trong oracle user là một tài khoản trong cơ sở dữ liệu và khi một user được tạo thì nó sẽ được phép đăng nhập và sở hữu một schema mỗi user chỉ sở hữu một schema và ngược lại.
- Schema là một tập hợp các đối tượng trong cơ sở dữ liệu được quản lý bởi một user nào đó. Các đối tượng của schema có thể là table, view, stored procedure, index, sequence ... Schema được tạo tự động khi user được tạo. 
