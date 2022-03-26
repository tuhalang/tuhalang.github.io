---
layout: post
title: Hướng dẫn cài đặt oracle 11g trên ubuntu sử dụng docker
categories: [Oracle, Database, Docker]
description: Hướng dẫn cài đặt oracle 11g trên ubuntu sử dụng docker
keywords: Database, Oracle, Docker
---

**Tóm tắt:** Trong bài viết này mình sẽ hướng dẫn các bạn cài đặt oracle database 11g chạy trên docker

I. Cài đặt docker
- Để cài đặt docker trên ubuntu thì vô cùng đơn giản bạn chỉ cần mở terminal và chạy câu lệnh sau: 
```bash
sudo apt-get install docker-ce
```

- Kiểm tra version docker vừa được cài đặt:
```bash
sudo docker --version
```

- Run hello world 
```bash
sudo docker run hello-world
```

II. Run oracle database

- Việc cài đặt oracle database bây giờ sẽ trở nên vô cùng đơn giản chúng ta chỉ cần mở terminal và chạy câu lệnh sau:

```bash
sudo docker run -d -p 1521:1521 epiclabs/docker-oracle-xe-11g
```
- Tuy nhiên nếu ta run oracle database như trên thì mỗi lần khởi động lại nó sẽ mất hết dữ liệu vì thế ta sẽ phải mount volumn:

```bash
sudo docker run -d -v <your folder>:/u01/app/oracle -p 1521:1521 epiclabs/docker-oracle-xe-11g
```
- Như vậy thông tin kết nối database sẽ như sau:

    |Thuộc tính|Giá trị|
    |----------|-------|
    |hostname|localhost|
    |port| 1521|
    |sid| xe|
    |username| system|
    |password| oracle|

- Trong trường hợp bạn không muốn sử dụng port 1521 mà muốn xử dụng port khác thì chỉ cần thay đổi như sau:

```bash 
sudo docker run -d -v <your folder>:/u01/app/oracle -p XXXX:1521 epiclabs/docker-oracle-xe-11g
```
