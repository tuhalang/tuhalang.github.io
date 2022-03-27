---
layout: post
title: Solidity foundation - Part 1
categories: [Smartcontract, Blockchain, Solidity]
description: Solidity foundation - Part 1
keywords: Smartcontract, Blockchain, Solidity
---

In this post, we will dicuss the basics of solidity language.

## I. What is Solidity? 

- Solidity is an object-oriented, high-level language for implementing smart contracts. Smart contracts are programs which govern the behaviour of accounts within the Ethereum state.

## II. Language description

### 1. Overview

- Solidity is quite similar with Java. You have to write classes in Java for everything, while you have to write contracts in Solidity for everything.

### 2. Layout of a Solidity source file

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

import "filename";

contract SimpleStorage {
    string public greet = "Hello World!";
}
```

- ***SPDX-License-Identifier: GPL-3.0*** : To build trust with the community, every source files of contract should be public, which lead to the legal problems with regards the copywrite. The solidity compiler encourages the use of machine-readable SPDX License Identifier, every source file should start with a comment:
    
    ```solidity
    // SPDX-License-Identifier: GPL-3.0
    ```

- ***pragma solidity >=0.4.16 <0.9.0*** : Specify compiler version.

- ***import "filename"*** : It is similar to other programming languages, using this to import other source files.

- ***contract SimpleStorage { }*** : Declare a contract, every logic of contract is described here. It's really similar with Java.

### 3. Primitive Data Types

- We introduce you to some primitive data types avaiable in Solidity.
- We will dicuss how to declare a variable later.

#### boolean:

- Declare:

    ```solidity
    boolean public boo;
    ```
- If you don't specify the value when declaring the variable, the default value will be assigned is **false**

#### uint:

- uint stands for unsigned integer. It have many sizes:
    
    - uint8 ranges from 0 to 2 ** 8 - 1
    - uint16 ranges from 0 to 2 ** 16 - 1
    - ...
    - uint256 ranges from 0 to 2 ** 256 - 1
- Declare:

    ```solidity
    uint8 public u8;
    uint16 public u16;
    uint public u; //uint is alias for uint256
    ```
- If you don't specify the value when declaring the variable, the default value will be assigned is **0**

#### int

- int stands for unsigned and signed interger. Like uint, different ranges are avaiable from int8 to int256:
    
    - int8 ranges from -2 ** 7 to 2 ** 7 - 1
    - int16 ranges from -2 ** 15 to 2 ** 15 - 1
    - ...
    - int256 ranges from -2 ** 255 to 2 ** 255 - 1

- Declare:

    ```solidity
    int8 public u8;
    int16 public u16;
    int public u; //int is same as int256
    ```
- If you don't specify the value when declaring the variable, the default value will be assigned is **0**

#### address

- address stands for address of account in ethereum. An address has 20 byte value size (160 bits or 40 hex characters). It corresponds to the last 20 bytes of the Keccak-256 hash of the public key

- Declare:

    ```solidity
    address public a;
    ```
- If you don't specify the value when declaring the variable, the default value will be assigned is **0x0000000000000000000000000000000000000000**

### 4. Variables

- There are three types of variables in Solidity:

    - Local: 

        - Declared inside a function
        - Not stored on the blockchain

    - State:

        - Declared outside a function
        - Stored on the blockchain

    - Golobal: provides information about the blockchain

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Variables {
    // State variables are stored on the blockchain.
    string public stateVariable = "Local Variable";

    function nothing() public {
        // Local variables are not saved to the blockchain.
        uint localVariable = 0;

        // Here are some global variables
        uint timestamp = block.timestamp; // Current block timestamp
        address sender = msg.sender; // address of the caller
    }
}
```

### 5. Constants

- Constants are variable which cannot be modified value.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Constants {
    uint public constant MY_CONSTANT = 123;
}
```

### 6. Immutable

- Immutable is variable which can be set inside the constructor but cannot be modified afterwards.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Immutable {
    // coding convention to uppercase constant variables
    uint public immutable MY_IMMUTABLE;

    constructor(uint _myUint) {
        MY_IMMUTABLE = _myUint;
    }
}
```

### 7. If/Else

- Like other programming languages, Solidity supports condition statement **If**, **else** and **ternary**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract IfElse {
    function ifElseStatement(uint x) public pure returns (uint) {
        if (x < 10) {
            return 0;
        } else if (x < 20) {
            return 1;
        } else {
            return 2;
        }
    }

    function ternary(uint _x) public pure returns (uint) {
        // if (_x < 10) {
        //     return 1;
        // }
        // return 2;

        // shorthand way to write if / else statement
        return _x < 10 ? 1 : 2;
    }
}
```

### 8. Loop

- Solidity supports both **for loop** and **while loop**
- Using **while loop** need attention to avoid infinite loop

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Loop {
    function loop() public {
        // for loop
        for (uint i = 0; i < 10; i++) {
            if (i == 3) {
                // Skip to next iteration with continue
                continue;
            }
            if (i == 5) {
                // Exit loop with break
                break;
            }
        }

        // while loop
        uint j;
        while (j < 10) {
            j++;
        }
    }
}
```