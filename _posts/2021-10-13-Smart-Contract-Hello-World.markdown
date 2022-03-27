---
layout: post
title: Smart contract - Hello world
categories: [Smartcontract, Blockchain]
description: Smart contract - Hello world
keywords: SmartContract, Blockchain
---

**Tóm tắt:** Trong bài viết này chúng ta sẽ cùng tìm hiểu về  cách để bắt đầu viết một smart contract đơn giản.

### 1. Smart contract (Hợp đồng thông minh) là gì ?

- Hợp đồng truyền thống
    - Hợp đồng là một cam kết giữa hai hay nhiều bên (pháp nhân) để làm hoặc không làm một việc nào đó trong khuôn khổ pháp luật.
    - Hợp đồng có thể thể hiện bằng văn bản hay lời nói nếu có người làm chứng, nếu vi phạm hợp đồng hay không theo cam kết thì 2 bên sẽ cùng nhau ra tòa và bên thua sẽ phải chịu mọi phí tổn.

- Như các bạn có thể thấy hợp đồng truyền thống cần có sự giằng buộc của pháp luật để đảm bảo các bên tham gia sẽ phải tuân thủ các điều khoản của hợp đồng.

- Khác với hợp đồng tuyền thống hợp đồng thông minh có thể đảm bảo các điều khoản được thực thi một các đúng đắn và tự động mà không có sự can thiệp từ bên ngoài nhờ công nghệ blockchain.

- Smart contract đơn giản là một chương trình máy tính chạy trên mạng blockchain. Nó là tập hợp của các hàm, các trạng thái của dữ liệu được lưu tại một địa chỉ cụ thể trên mạng blockchain.

### 2. Tạo một smart contract đơn giản.

- Môi trường phát triển (IDE): Trong loạt bài viết này mình sẽ sử dụng [Remix](https://remix.ethereum.org/)

![Remix IDE](https://tuhalang.github.io/assets/smart_contract/remix_ide.png)

- Ngôn ngữ lập trình để tạo smart contract được gọi là [Solidity](https://docs.soliditylang.org/en/v0.8.9/)

- Đầu tiên các bạn tạo file contracts/PingPong.sol trên giao diện của Remix

```solidity
pragma solidity >=0.7.0 <0.9.0;

contract PingPong {
    
    string pong;
    
    constructor(){
        pong = "pong";
    }
    
    function ping() public view returns (string memory){
        return pong;
    }
}
```
![Remix Code](https://tuhalang.github.io/assets/smart_contract/remix_code.png)

Tiếp theo bạn chuyển sang tab compiler để compiler code

![Remix compiler](https://tuhalang.github.io/assets/smart_contract/remix_compiler.png)

Tiếp đến chuyển qua tab deploy

![Remix deploy](https://tuhalang.github.io/assets/smart_contract/remix_deploy.png)

Sau khi deploy thành công bạn có thể test thử bằng cách gọi hàm **ping()** khi đó sẽ trả về kết quả **pong**

![Remix test](https://tuhalang.github.io/assets/smart_contract/remix_test.png)

**Trong phần tiếp theo mình sẽ giải thích cụ thể các thanh phần trong Remix IDE cũng như các thành phần của một chương trình Solidity**