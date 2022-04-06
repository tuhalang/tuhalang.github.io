---
layout: post
title: Visibility in Solidity
categories: [Smartcontract, Blockchain, Solidity]
description: Call, DelegateCall in Solidity
keywords: Smartcontract, Blockchain, Solidity, Visibility
---

In Solidity, when you want to call other contracts, you can use **call** or **delegateCall** functions. So, what are different between **call** and **delegateCall**?

### Call vs DelegateCall

- The main difference between them is that while call just executes the function in the context of the contract it was defined, delegatecall inherits the execution context, meaning that the function will behave as it was defined in the contract that's using delegatecall.

- For example:

    - Call:

    ![call_function](https://tuhalang.github.io/assets/call_delegate_call/call_diagram.svg)

    - DelegateCall:

    ![deletage_call_function](https://tuhalang.github.io/assets/call_delegate_call/delegate_call_diagram.svg)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Reciever {
    uint public x;
    uint public z;
    uint public y;

    function setX(uint _x) public {
        x = _x;
    }

    function setY(uint _y) public {
        y = _y;
    }
}

contract Caller {
    uint public x;
    uint public y;

    function useCall(address _contract, uint _x, uint _y) public {
        (bool successX, ) = _contract.call(
            abi.encodeWithSignature("setX(uint256)", _x)
        );
        require(successX, "Error");

        (bool successY, ) = _contract.call(
            abi.encodeWithSignature("setY(uint256)", _y)
        );
        require(successY, "Error");
    }

    function useDelegateCall(address _contract, uint _x, uint _y) public {
        (bool successX, ) = _contract.delegateCall(
            abi.encodeWithSignature("setX(uint256)", _x)
        );
        require(successX, "Error");

        (bool successY, ) = _contract.delegateCall(
            abi.encodeWithSignature("setY(uint256)", _y)
        );
        require(successY, "Error");
    }
}
```

- If you call function ***useCall()***, the value of x and y vatiables in Recevier contract will be changed.
- If you call function ***useDelegateCall()***, the value of x variable and storage in slot 2 in Caller contract will be change. 
- Why not the variable y but the storage in slot 2? 

    - Look back a bit on the layout of storage in solidity, variables will be stored in slots 0, 1, 2, ... respectively.
    - Because the variable y in Reciever contract is stored in slot 2, when using delegateCall to call, the corresponding variable stored in slot 2 of Caller contract will be changed.

***You need to be really careful with storing conflicts when using delegateCall.***

