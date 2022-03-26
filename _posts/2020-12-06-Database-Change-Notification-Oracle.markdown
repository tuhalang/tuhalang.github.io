---
layout: post
title: Database Change Notification Oracle
categories: [Oracle, Database]
description: Database Change Notification Oracle
keywords: Database, Oracle
---

**Tóm tắt:** Trong bài viết này chúng ta sẽ cùng tìm hiểu về **Database Change Notification Oracle** hoạt động như thế nào và khi nào thì chúng ta nên sử dụng nó?

#### 1. Database Change Notification Oracle là gì?

- Đây là một tính năng của database Oracle được sử dụng để thông báo các thay đổi của các bản ghi trong database

- Thông báo sẽ được đẩy về sau khi transaction được commit

- JDBC driver có thể đăng kí câu lệnh truy vấn với database và nhận thông báo phản hồi theo các trường hợp sau:

    + DML hoặc DDL thay đổi một đối tượng liên kết với câu truy vấn 
    
    + DML hoặc DDL thay đổi kết quả của câu truy vấn

#### 2. Các bước thực hiện để đăng kí nhận thông báo

- Registration: thực hiện đăng kí nhận thông báo từ database

- Query association: sau khi thực hiện đăng kí, chúng ta cần thực hiện liên kết các câu truy vấn với nó. Các câu truy vấn này là một phần của đăng kí

- Notification: thông báo được tạo để đáp ứng lại những thay đổi từ bảng hoặc kết kết quả theo câu truy vấn. Oracle sẽ gửi các thông báo này thông qua JDBC driver

#### 3. Database Change Notification được dùng trong trường hợp nào?

- Bản thân mình khi gặp được một task ở công ty là quét các thay đổi của bảng log trong database Oracle để xử lý logic nghiệp vụ đã nghĩ đến việc sử dụng Oracle Change Notification tuy nhiên sau khi tìm hiểu thì đã thấy nó không phù hợp cho việc này bởi các vì:

    + Các sự kiện được thông báo về theo cơ chế đồng bộ (sự kiện trước được xử lý xong thì sự kiện sau mới được thông báo tiếp)

    + Trong thông báo chỉ cho chúng ta thông tin về rowid mà không bao gồm cả dòng dữ liệu mà chúng ta sẽ phải query lại để lấy dữ liệu

    + Mỗi lần chỉ trả về thông tin của một bản ghi thay đổi

=> Do bảng log sẽ được cập nhật liên tục nên việc xử lý các sự kiện tuần tự và phải query lại mỗi bản ghi cho mỗi lần xử lý sẽ làm giảm hiệu năng của hệ thống

- Vì thế như khuyến cáo từ document của Oracle thì Database Change Notification chỉ nên dùng cho các trường hợp dữ liệu thay đổi ít như để cập nhật thông tin cho cache khi có thay đổi từ database.