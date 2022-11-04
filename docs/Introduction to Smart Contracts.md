# 介绍智能合约

[一个简单的只能合约](#a_sample_contract)


## 一个简单的智能合约{#a_Sample_Contract}

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

下面一行的意思是说这段源代码用Solidity 0.4.16版本或更高的版本编写的，但最高不超过0.9.0版本。这行代码确保这个合约不会被更高版本的编译器所编译，因为这些编译器的行为可能有所不同。[Pragmas](./Pragma.md)是告诉编译器怎么样去处理源代码的常用指令（比如：Pragma once）。

Solidity实质意义的合约是一个保存在以太坊区块链上的特定地址代码（功能）和数据（状态）的集合。
<kbd style="color:red">unit storedData;</kbd> 这一行，声明了一个类型是uint(256位无符号整形)叫<kbd style="color:red"> storedData</kbd>状态变量。你可以认为它是一个数据库里插槽。你可以通过调用管理数据的代码来查询和变更它。在这个例子中，智能合约定义了<kbd style="color:red">set</kbd>和<kbd style="color:red">get</kbd>两个函数，用来修改或检索这个变量的值。

访问当前合约的个成员(如状态变量)的时候，通常添加<kbd style="color:red">this.</kbd>前缀，你只需要直接通过成员的名字访问它。与其他一些语言不同，忽略它不仅是一个样式问题，它会导致成员访问方式完全不同，稍后会详细介绍。

除了(由于以太坊构建的基础设施)允许任何人存储世界上任何人都可以访问的单个数字之外，这份智能合约还没有做太多的事情，没有任何(可行的)方式可以阻止你发布这个数字。任何人可以调用<kbd style="color:red">set</kbd>方法，用不同的值覆盖你的数字，但是这个数字仍然存在区块链的历史数据中。稍后，你可以看到如何增加访问限制，以保证只有你可以修改这个数字。

<div style="background:#e9b684;color:#fff;padding:5px;font-weight:600;">
⚠️ Waring
</div>
<div style="background:#fdeecf;color:#404040;padding:10px">
小心使用Unicode文本，那些看着像(甚至是相同的)字符，可能有不同的码位，因此被编码为不同的自己阵列。
</div>

<br/>

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
所有的是识标识符（合约名称，函数名，变量名）都被存储为ASCII字符集。可以将UTF-8编码的数据存储在字符串变量中。
</div>


