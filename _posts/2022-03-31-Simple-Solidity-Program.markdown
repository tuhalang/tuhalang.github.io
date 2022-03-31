---
layout: post
title: A simple solidity program
categories: [Smartcontract, Blockchain, Solidity]
description: A simple solidity program
keywords: Smartcontract, Blockchain, Solidity
---

In this post, we will write code and test a simple solidity program.

### 1. Prepare environment

- Text eiditor: [vscode](https://code.visualstudio.com/)
- Framework: [truffle](https://trufflesuite.com/docs/truffle/getting-started/installation.html)

### 2. Write code

- We will write a Counter program with four functions:
    - Increase counter
    - Descrease counter
    - Retrieve counter
    - Reset counter

#### Step 1: Init project

```bash
# create Counter folder
mkdir Counter
cd Counter

# Init project
truffle init
```

- The directory structure should be as follows:

```
Counter
│   package.json
│   truffle-config.js   
│
|───contracts
│       Migrations.sol
│
|───migrations
|       1_initial_migration.js
|
|___test

```


#### Step 2: Write contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract Counter {
    uint public count;

    modifier geaterThanZero () {
        require(count > 0, "impossiable decrease counter");
        _;
    }

    constructor() {
        count = 0;
    }

    function get() public view returns (uint) {
        return count;
    }

    function inc() public {
        count += 1;
    }

    function dec() geaterThanZero public {
        count -= 1;
    }

    function reset() public {
        count = 0;
    }
}
```

### 3. Testing

#### Step 1: Create file Counter.js in test folder 

```bash
touch test/Counter.js
```

#### Step 2: Write test case

```js
const Counter = artifacts.require('Counter');

contract("Counter", async accounts => {

    let counter;
    beforeEach(async () => {
        counter = await Counter.deployed();
    })

    // count: 0 -> 1
    it("inc", async () => {
        const before = await counter.get();
        await counter.inc();
        const after = await counter.get();
        assert.equal(parseInt(before) + 1, parseInt(after));
    })

    // count: 1 -> 0
    it("dec", async () => {
        const before = await counter.get();
        await counter.dec();
        const after = await counter.get();
        assert.equal(parseInt(after) + 1, parseInt(before));
    })

    // count: 0 -> 1 -> reset -> 0
    it("reset", async () => {
        await counter.inc();
        const before = await counter.get();
        await counter.reset();
        const after = await counter.get();
        assert.notEqual(parseInt(before), 0);
        assert.equal(parseInt(after), parseInt(0));
    })

    // count: 0 -> error
    it("dec error", async () => {
        try {
            await counter.dec();
            throw new Error("Transaction did not fail !")
        } catch (error) {
            assert.equal(error.reason, "impossiable decrease counter")
        }
    })
})
```

#### Step 3: Run test

```bash
truffle test
```

The output should be as follows:
```
  Contract: Counter
    ✓ inc (118ms)
    ✓ dec (102ms)
    ✓ reset (152ms)
    ✓ dec error (409ms)
```

Next, we need a statistic about the percent coverage of the test case.
#### Step 4: Run test coverage

Install library

```bash
npm install --save-dev solidity-coverage
```

Edit file truffle-config.js

```
networks: {
    ...,
    plugins: ["solidity-coverage"],
    ...
}
```

Run test coverage

```bash
truffle run coverage --file="test/Counter.js"
```

And the final result here:
```
--------------|----------|----------|----------|----------|----------------|
File          |  % Stmts | % Branch |  % Funcs |  % Lines |Uncovered Lines |
--------------|----------|----------|----------|----------|----------------|
 contracts/   |      100 |      100 |      100 |      100 |                |
  Counter.sol |      100 |      100 |      100 |      100 |                |
--------------|----------|----------|----------|----------|----------------|
All files     |      100 |      100 |      100 |      100 |                |
--------------|----------|----------|----------|----------|----------------|
```

That is pretty good when we covered 100% line code :v