---
layout: post
title: Layout of state variables in storage, memory
categories: [Smartcontract, Blockchain, Solidity]
description: Layout of state variables in storage, memory
keywords: Smartcontract, Blockchain, Solidity
---

How to the storage or memory save state variables of the contract? In this post, we will discover it together.

### 1. Layout of state variables in storage

- Firstly, each storage slot stores 32 bytes data. Except for dynamically-size arrays and mappings, data is stored contiguously item after item starting with the first state variable, which is stored in slot **0**.
- Multiple, contiguous items that need less than 32 bytes are packed into a single storage slot if possible, according to the following rules:
    - The first item in a storage slot is stored lower-order aligned.
    - Value types use only as many bytes as are necessary to store them.
    - If a value type does not fit the remaining part of a storage slot, it is stored in the next storage slot.
    - Structs and array data always start a new slot and their items are packed tightly according to these rules.
    - Items following struct or array data always start a new storage slot.
- For intance, two variables **uint128** can be stored in the same slot.

- ***Note***: 
    - When using elements are smaller than 32 bytes, your contract's gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. 
    - However, you also can take advantage of this to speed up processing. If you save many variables in the same slot and you need all of them for processing, you can combine multiple reads or writes into a single operation to save time.
    - For example, in a struct, declaring your storage variables in the order of **int128**, **int128**, **int256** is better than the order of **int128**, **int256**, **int128**. Because the former will only take up two slots of storage whereas the latter will take up three. Furthermore, all elements in a struct often are processed together, the former will only take up two operations to read or write whereas the latter will take three.

- The value corresponding to a array is located at **keccak256(p)**.

- For intance, the location of **x[i][j]**, where the type of **x** is **uint24[][]**, is computed as follows (assuming **x** itself is stored at slot **p**):
    - The slot is **keccak256(keccak256(p) + i) + floor(j / floor(256 / 24))**
    - **x**       -> **p**
    - **x[0]**    -> **keccak256(p)**
    - **x[i]**    -> **keccak256(p) + i**
    - **x[i][0]** -> **keccak256(keccak256(p) + i**)
    - **x[i][j]** -> **keccak256(keccak256(p) + i) + floor(j / floor(256 / 24))**

- The value corresponding to a mapping key k is located at **keccak256(h(k) . p)** where **.** is concatenation and h is a function that is applied to the key depending on its type:
    - for value types, h pads the value to 32 bytes in the same way as when storing the value in memory.
    - for strings and byte arrays, h(k) is just the unpadded data.

- For intance, 

    ```solidity
    mapping(uint => mapping(uint => uint)) data;
    ```
- The location of data[i][j] (assuming the position of the mapping itself is **p**) is: **keccak256(uint256(j) . keccak256(uint256(i) . p))**:
    - **data[i]**    -> **keccak256(uint256(i) * p)**
    - **data[i][j]** -> **keccak256(uint256(j) * keccak256(uint256(i) * p))**

### 2. Layout of state variables in memory
- Elements in memory arrays in Solidity always occupy multiples of 32 bytes
- Solidity reserves four 32-byte slots, with specific byte ranges (inclusive of endpoints) being used as follows:
    - 0x00 - 0x3f (64 bytes): scratch space for hashing methods
    - 0x40 - 0x5f (32 bytes): currently allocated memory size (free memory pointer)
    - 0x60 - 0x7f (32 bytes): zero slot   