# 介绍智能合约

## 一个简单的智能合约

我们从一个基础例子开始。这个例子为变量赋值，并将其公开以供其他合约访问。如果你现在还不能理解所有内容也没关系，我们会后面会做更详细的讲解。

### 存储的例子

``` C++

// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}

```

上面代码第一行，告诉我们这段代码在GPL version 3.0协议下获得许可。一个可以被编译器识别的许可说明符在源代码发布的设置中非常重要。您可以从[这里](https://spdx.org/licenses/)查看这些协议内容。

<kdb>abc</kdb>