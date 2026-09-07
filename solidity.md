
# 9

- assert(bool condition): abort execution and revert state changes if condition is false (use for internal error)
- require(bool condition): abort execution and revert state changes if condition is false (use for malformed input or error in external component)
- require(bool condition, string memory message): abort execution and revert state changes if condition is false (use for malformed input or error in external component). Also provide error message.
- revert(): abort execution and revert state changes
- revert(string memory message): abort execution and revert state changes providing an explanatory string

# 8

```
fallback execute when
    - function doesn't exit
    - directly send ETH

fallback() or receive()?

    Ether is sent to contract
                |
        is msg.data empty?
               / \
receive() exist?   fallback()
        / \
      yes  no
      /     \
receive()   fallback()
```

# 7

`string`、`bytes`、数组、结构体这类**引用类型**，外部函数参数必须显式指定数据位置：`memory` / `calldata`，不能省略。

# 6

create contract form memory

```
assembly{
    // create(v, p, n)
    // v = amout of ETH to send
    // p = pointer in memory to start of code
    // n = size of code

    addr := create(callvalue(), add(_code, 0x20), mload(_code))
}
```

# 5

- msg
  - msg.sender
  - msg.value
  - msg.data
  - msg.sig
- tx
  - tx.origin
  - tx.gasprice
- block
  - block.number
  - block.timestamp
  - block.coinbase
  - block.gaslimit
  - block.basefee
  - block.chainid

# 4

[etherscan](etherscan.io)

# 3

[Solidity 0.8](https://www.youtube.com/playlist?list=PLO5VPQH6OWdVQwpQfw9rZ67O6Pjfo6q-p)

# 2

[documentation](https://docs.soliditylang.org/)

# 1

[Remix IDE](https://remix.ethereum.org)
