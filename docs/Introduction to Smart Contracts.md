# 介绍智能合约

## 一个简单的智能合约<span id="easy_contract"></span>

我们从一个基础例子开始。这个例子为变量赋值，并将其公开以供其他合约访问。如果你现在还不能理解所有内容也没关系，我们会后面会做更详细的讲解。

### 存储的例子<span id="store_example"></span>

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

<br/>


### 子货币示例

下面这个合约实现了最简单的加密货币形式。这个合约只允许合约的创建人创建新货币(可能用不同的发行方案)。任何人可以相互发送货币不需要使用用户名和密码注册。你需要的全部只是一对以太坊密钥对。

```C++

// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping (address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}

```

这个合约介绍一些新概念，让我们一个一个的了解它们。

```address public minter``` 这行定义了一个[地址](./Address.md)类型的状态变量。```address```类型是一个不允许任何算数运算的160位的值。它适用于存储合约地址或者一个[外部账户](./ExternalAccounts.md)公钥的哈希。

关键词```public```自动生成一个允许你从合约外部访问当前状态变量值的函数。没有这个关键词，其他合约没法访问这个变量。由编译器生成的函数代码等价于下面这段代码(暂时忽略[外部](./External.md)和[视图](./View.md)):

```C++

function minter() external view returns(address) {return minter;}

```

你可以自己添加一个像上面那样函数，但是你会获得一个同名的函数和状态变量。你不需要这么做，编译器会为你计算出来。

下一行，```mapping (address => uint) public balances;``` 还是创建一个公共状态变量，但它是个更复杂的数据类型。这个[映射](./Mapping.md)类型把地址映射到一个[无符号的整形数](./UnsignedIntegers.md)。

映射可以被看做是一个虚拟初始化的[哈希表](./HashTable.md)。这个表里，每个可能键从一开始就存在，并映射到一个字节表示全为0的值。然而，既不能，获得映射的所有键的列表，也不能获得所有值的列表。记录你添加到映射的内容，或者不需要的在上下文中使用它。

或者更好的是，维护一个列表或使用更合适的数据类型。


在映射情况下，由```public```关键词创建的[getter function](./GetterFunction.md)更复杂。看下面这个例子：

```

function balances(address account) external view returns (uint){
    return balances[account];
}

```

你可以使用上面这个函数查询一个账户的余额。

```send```函数里最后一行，``` evernt Sent(address from, address to, uint amount) ``` 这行声明了一个[事件](./Event.md)。以太坊客户端(例如web应用)，可以不需太大成本，监听区块链上发出的这些事件。这些事件一发出，监听器收到参数```from```,```to```和```amonut```,从而跟踪交易成为可能。

监听这些事件，你可以用下面的JavaScript代码，这段代码用web3.js创建"币"合同对象，并且任何用户接口都会从上面自动调用生成```balance```函数。


```

Coin.Sent().watch({}, '', function(error, result) {
    if (!error) {
        console.log("Coin transfer: " + result.args.amount +
            " coins were sent from " + result.args.from +
            " to " + result.args.to + ".");
        console.log("Balances now:\n" +
            "Sender: " + Coin.balances.call(result.args.from) +
            "Receiver: " + Coin.balances.call(result.args.to));
    }
})

```

[构造函数](./Constructor.md)是一个特殊的函数，它在合约创建的时候被执行，且之后不能再被调用。在这个例子中，它永久存储创建这个合约人的地址。变量```msg```（```tx```和```block```一样）是一个特殊的全局变量，包含允许访问区块链的属性。```msg.sender``` 始终是当前(外部)函数调用的地址。

这些组成合约的函数，和那些用户与合约可以调用的函数是```mint```和```send```。


```mint```函数向另外一个地址发送最新铸造的“币”数量。这个[require](./Require.md)函数定义如果不满足条件的情况下恢复所有更改的条件。在这个例子中，```require(msg.sender == minter);```确保只有合约的创建人才能调用```mint```函数。通常情况下，合约创建人可以按照他们的意愿铸造任意多个tokens，但在某种程度上，这会导致“溢出(overflow)”的现象。需要注意的是，因为默认的[检查算法](./CheckedArithmetic.md)，如果表达式```balances[receiver]+=amount```溢出，交易会被退回。例如：当```balances[receiver]+amount``` 任意精度计算中大于 uint的最大值(2**256-1）的时候。在函数```send```的对账单balances[receiver]+=amount里也一样。


[Errors](./Errors.md)允许你给调用者提供更多关于一个条件或操作失败原因的信息。[Errors](./Errors.md)通常和[revert 语句](./Revert.md)在一起使用。```revert```语句无条件终止并恢复所有和```require```函数一样的变更，而且他还允许你提供一个错误的名字和附加你想要提供给调用者(并最终提供给前端程序或块资源管理器)的数据，以便失败更容易被调试或对其做出反应。


任何(已经有"币")的人可以使用```send```函数把“币”发给另外一个人。如果这个发送者没有足够的“币”去发送，```if```条件将计算为True。```revert```会导致操作失败，同时使用```InsufficientBalance```错误向发送者提供错误详细信息。

<br/>

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
如果你使用智能合约向一个地址发送"币"，当你在区块链管理器上查找这个地址的时候，你会一无所获，因为你发币和调整余额的这条记录仅被存储在这个特定"币"合同的数据区。通过使用事件，你可以创建爱你一个区块链管理器，用来跟踪交易和你新"币"的余额，但是你必须检查“币”合约地址而不是这个“币”所有者的地址。
</div>

### 区块链基础