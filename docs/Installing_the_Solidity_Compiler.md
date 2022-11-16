# 安装Solidity编译器

## 版本控制

Solidity版本遵循[Semantic_Versioning](https://semver.org/)规则。此外，主版本 0（即 0.x.y）的补丁级别版本将不包含重大更改。也就是说0.x.y版本编译的代码，可以被0.x.z编译。（其中z>y）

除了发布之外，我们还提供夜间开发版本，目的是让开发人员更容易尝试即将推出的功能并提供早期反馈。但是请注意，虽然夜间构建通常非常稳定，但它们包含来自开发分支的前沿代码，并且不能保证始终有效。尽管我们尽了最大努力，但它们可能包含未记录和/或损坏的更改，这些更改不会成为实际版本的一部分。它们不适用于生产用途。

部署合约时，您应该使用最新发布的 Solidity 版本。这是因为定期引入重大更改以及新功能和错误修复。我们目前使用 0.x 版本号来表示这种快速变化。


## Remix

我们推荐 Remix 用于小型合约和快速学习 Solidity。

在线访问 [Remix](https://remix.ethereum.org)，您无需安装任何东西。如果您想在不连接 Internet 的情况下使用它，请转到 https://github.com/ethereum/remix-live/tree/gh-pages 并按照该页面上的说明下载``` .zip``` 文件。 Remix 也是无需安装多个 Solidity 版本即可测试夜间构建的便捷选项。

此页面上的更多选项详细说明了在您的计算机上安装命令行 Solidity 编译器软件。如果您正在处理更大的合同或需要更多编译选项，请选择命令行编译器。


## npm / Node.js

使用npm，以方便且便携式的方式安装```Solcjs```。 SOLCJS程序的功能少于访问此页面下方描述的编译器的方法。使用[命令行编译器](./Commandline_Compiler.md)文档假定您正在使用全功能编译器Solc。 SOLCJS的用法记录在其自己的[git库](https://github.com/ethereum/solc-js)中。

``` NodeJS

npm install -g solc

```

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
命令行可执行文件命名为```SOLCJS```。
SOLCJS的命令行选项与Solc和工具（例如Geth）不兼容，期望Solc的行为不适用于Solcjs。
</div>

<br/>

## Docker

Solidity 构建的 Docker 镜像可使用以太坊组织的 solc 镜像获得。对最新发布的版本使用 stable 标签，对于开发分支中潜在的不稳定更改使用 nightly 标签。
Docker 映像运行编译器可执行文件，因此您可以将所有编译器参数传递给它。例如，下面的命令会拉取 solc 映像的稳定版本（如果您还没有），并在新容器中运行它，并传递 --help 参数。

```
docker run ethereum/solc:stable --help

```

要使用 Docker 镜像在主机上编译 Solidity 文件，请挂载本地文件夹用于输入和输出，并指定要编译的合约。例如。

```
docker run -v /local/path:/sources ethereum/solc:stable -o /sources/output --abi --bin /sources/Contract.sol
```

您还可以使用标准 JSON 接口（在使用带有工具的编译器时推荐）。使用此接口时，只要 JSON 输入是自包含的（即它不引用必须由导入回调加载的任何外部文件），就不需要挂载任何目录。

```
docker run ethereum/solc:stable --standard-json < input.json > output.json
```

## Linux Packages

Solidity 的二进制包可在solidity/releases 获得。
我们还有适用于 Ubuntu 的 PPA，您可以使用以下命令获取最新的稳定版本：

```
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install solc

```

可以使用以下命令安装 nightly 版本：

```
sudo add-apt-repository ppa:ethereum/ethereum
sudo add-apt-repository ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install solc
```

此外，一些 Linux 发行版提供自己的软件包。这些包不由我们直接维护，但通常由各自的包维护者保持最新。
例如，Arch Linux 有最新开发版本的软件包：

```
pacman -S solidity

```

还有一个 snap 包，但是，它目前未维护。它可以安装在所有受支持的 Linux 发行版中。要安装最新的稳定版 solc：

```
sudo snap install solc

```

如果您想帮助测试最新开发版本的 Solidity，请使用以下内容：

```
sudo snap install solc --edge

```

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
```solc``` snap 使用严格限制。这是 snap 包最安全的模式，但它有一些限制，比如只能访问 ```/home``` 和 ```/media``` 目录中的文件。有关更多信息，请访问揭秘 [Snap Confinement](https://snapcraft.io/blog/demystifying-snap-confinement)。
</div>

<br/>

## MacOS 包

我们通过 Homebrew 分发 Solidity 编译器作为源代码构建版本。目前不支持预制瓶。

```
brew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
```

要安装最新的 0.4.x / 0.5.x 版本的 Solidity，您还可以分别使用 ```brew install solidity@4``` 和 ```brew install solidity@5```。

如果您需要特定版本的 Solidity，您可以直接从 Github 安装 Homebrew 公式。提交到Github的[solidity.rb](https://github.com/ethereum/homebrew-ethereum/commits/master/solidity.rb)

复制您想要的版本的提交哈希并在您的机器上检查它。

```
git clone https://github.com/ethereum/homebrew-ethereum.git
cd homebrew-ethereum
git checkout <your-hash-goes-here>

```

使用```brew```安装

```
brew unlink solidity
# eg. Install 0.4.8
brew install solidity.rb
```

## 静态二进制

我们在 [solc-bin](https://github.com/ethereum/solc-bin/) 维护一个存储库，其中包含所有受支持平台的过去和当前编译器版本的静态构建。这也是您可以找到Nightly构建的位置。

该存储库不仅是最终用户获取开箱即用的二进制文件的一种快速简便的方法，而且它对第三方工具也很友好：

- 内容镜像到 [https://binaries.soliditylang.org](https://binaries.soliditylang.org)，可以通过 HTTPS 轻松下载，无需任何身份验证、速率限制或使用 git。

- 内容使用正确的 Content-Type 标头和宽松的 CORS 配置提供，以便可以由浏览器中运行的工具直接加载。

- 二进制文件不需要安装或解包（与必要的 DLL 捆绑在一起的旧 Windows 版本除外）。

- 我们力求实现高水平的向后兼容性。文件一旦添加，如果不在旧位置提供符号链接/重定向，就不会被删除或移动。它们也永远不会被修改，并且应该始终与原始校验和匹配。唯一的例外是损坏或无法使用的文件，如果保持原样，可能弊大于利。

- 文件通过 HTTP 和 HTTPS 提供。只要您以安全的方式（通过 git、HTTPS、IPFS 或仅将其缓存在本地）获取文件列表并在下载二进制文件后验证它们的哈希值，您就不必对二进制文件本身使用 HTTPS。

大多数情况下，Github 上的 Solidity 发布页面上都提供了相同的二进制文件。不同的是，我们一般不会在 Github 发布页面上更新旧版本。这意味着如果命名约定发生更改，我们不会重命名它们，并且我们不会为发布时不支持的平台添加构建。这只发生在```solc-bin``` 中。

```solc-bin``` 存储库包含几个顶级目录，每个目录代表一个平台。每个都包含一个 list.json 文件，其中列出了可用的二进制文件。例如，在 ```emscripten-wasm32/list.json``` 中，您将找到有关 0.7.4 版本的以下信息：

```
{
  "path": "solc-emscripten-wasm32-v0.7.4+commit.3f05b770.js",
  "version": "0.7.4",
  "build": "commit.3f05b770",
  "longVersion": "0.7.4+commit.3f05b770",
  "keccak256": "0x300330ecd127756b824aa13e843cb1f43c473cb22eaf3750d5fb9c99279af8c3",
  "sha256": "0x2b55ed5fec4d9625b6c7b3ab1abd2b7fb7dd2a9c68543bf0323db2c7e2d55af2",
  "urls": [
    "bzzr://16c5f09109c793db99fe35f037c6092b061bd39260ee7a677c8a97f18c955ab1",
    "dweb:/ipfs/QmTLs5MuLEWXQkths41HiACoXDiH8zxyqBHGFDRSzVE5CS"
  ]
}
```


这意味着：

- 您可以在名称 solc-emscripten-wasm32-v0.7.4+commit.3f05b770.js 下的同一目录中找到二进制文件。请注意，该文件可能是符号链接，如果您不使用 git 下载它或您的文件系统不支持符号链接，则需要自行解析。

- 该二进制文件也反映在 https://binaries.soliditylang.org/emscripten-wasm32/solc-emscripten-wasm32-v0.7.4+commit.3f05b770.js。在这种情况下，不需要 git 并且符号链接可以通过提供文件副本或返回 HTTP 重定向来透明地解析。

- 该文件也可在 IPFS 上的 QmTLs5MuLEWXQkths41HiACoXDiH8zxyqBHGFDRSzVE5CS 获得。

- 该文件将来可能会在 Swarm 上的 16c5f09109c793db99fe35f037c6092b061bd39260ee7a677c8a97f18c955ab1 上可用。

- 您可以通过将其 keccak256 哈希与 0x300330ecd127756b824aa13e843cb1f43c473cb22eaf3750d5fb9c99279af8c3 进行比较来验证二进制文件的完整性。可以使用 sha3sum 提供的 keccak256sum 实用程序或 JavaScript 中 ethereumjs-util 中的 keccak256() 函数在命令行上计算哈希值。

- 您还可以通过将其 sha256 哈希与 0x2b55ed5fec4d9625b6c7b3ab1abd2b7fb7dd2a9c68543bf0323db2c7e2d55af2 进行比较来验证二进制文件的完整性。



<div style="background:#e9b684;color:#fff;padding:5px;font-weight:600;">
⚠️ Waring
</div>
<div style="background:#fdeecf;color:#404040;padding:10px">
由于强大的向后兼容性要求，存储库包含一些遗留元素，但在编写新工具时应避免使用它们：

- 如果您想要获得最佳性能，请使用 emscripten-wasm32/（回退到 emscripten-asmjs/）而不是 bin/。在 0.6.1 版本之前，我们只提供 asm.js 二进制文件。从 0.6.2 开始，我们切换到性能更好的 WebAssembly 构建。我们已经为 wasm 重建了旧版本，但原始 asm.js 文件仍保留在 bin/ 中。新的必须放在单独的目录中以避免名称冲突。

- 如果您想确定下载的是 wasm 还是 asm.js 二进制文件，请使用 emscripten-asmjs/ 和 emscripten-wasm32/ 代替 bin/ 和 wasm/ 目录。

- 使用 list.json 代替 list.js 和 list.txt。 JSON 列表格式包含旧版本的所有信息以及更多信息。

- 使用 https://binaries.soliditylang.org 而不是 https://solc-bin.ethereum.org。为了简单起见，我们将几乎所有与编译器相关的内容都移到了新的soliditylang.org 域下，这也适用于solc-bin。虽然建议使用新域，但仍完全支持旧域并保证指向同一位置。
</div>

<br/>

<div style="background:#e9b684;color:#fff;padding:5px;font-weight:600;">
⚠️ Waring
</div>
<div style="background:#fdeecf;color:#404040;padding:10px">
二进制文件也可在 https://ethereum.github.io/solc-bin/ 获得，但此页面在 0.7.2 版发布后停止更新，不会收到任何平台的任何新版本或每晚构建，并且确实不提供新的目录结构，包括非 emscripten 构建。

如果您正在使用它，请切换到 https://binaries.soliditylang.org，这是一个直接替换。这使我们能够以透明的方式对底层托管进行更改并最大限度地减少中断。与我们无法控制的 ethereum.github.io 域不同，binaries.soliditylang.org 可以保证长期工作并保持相同的 URL 结构。
</div>

<br/>

## 源代码构建

### 先决条件 - 所有操作系统

以下是所有 Solidity 构建的依赖项：

| 软件 | 备注 |
| :----: | :----: | 
| CMake (版本3.13+) | 跨平台生成文件生成器。 |
| Boost (Windows上为1.77+版，其他情况下为1.65+版) | C++库。 |
| Git | 用于检索源代码的命令行工具。 |
| z3 (版本4.8+，可选) | 用于SMT检查器。 | 
| cvc4 （可选） | 用于SMT检查器。 |


<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
0.5.10 之前的 Solidity 版本可能无法正确链接到 Boost 1.70+ 版本。一种可能的解决方法是在运行 cmake 命令配置 solidity 之前临时重命名<font color="red">Boost 安装路径/lib/cmake/Boost-1.70.0</font>。

从 0.5.10 开始链接到 Boost 1.70+ 应该无需人工干预即可工作。
</div>

<br/>

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
默认构建配置需要特定的 Z3 版本（代码最后更新时的最新版本）。 Z3 版本之间引入的更改通常会导致返回的结果略有不同（但仍然有效）。我们的 SMT 测试不考虑这些差异，并且可能会因与它们编写的版本不同的版本而失败。这并不意味着使用不同版本的构建是错误的。如果将 <font color="red">-DSTRICT_Z3_VERSION=OFF</font> 选项传递给 CMake，则可以使用满足上表中给出的要求的任何版本进行构建。但是，如果您这样做，请记住将<font color="red"> --no-smt</font> 选项传递给<font color="red"> scripts/tests.sh </font>以跳过 SMT 测试。
</div>

<br/>

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
默认情况下，构建以<i>迂腐模式</i>执行，这会启用额外警告并告诉编译器将所有警告视为错误。这迫使开发人员在警告出现时进行修复，这样它们就不会累积起来“稍后修复”。如果您只对创建发布版本感兴趣并且不打算修改源代码来处理此类警告，则可以将<font color="red"> -DPEDANTIC=OFF </font>选项传递给 CMake 以禁用此模式。不建议将此用于一般用途，但在使用我们未测试或尝试使用较新工具构建旧版本的工具链时可能是必要的。如果您遇到此类警告，请考虑<a href="https://github.com/ethereum/solidity/issues/new">报告</a>它们。
</div>

<br/>

#### 最低编译器版本

以下C++编译器和它的最低版本可以构建Solidity代码库：

* GCC，version 8+

* Clang，version 7+

* MSVC， version 2019+

### MacOS 先决条件

对于 macOS 构建，请确保安装了最新版本的 Xcode。这包含在 OS X 上构建 C++ 应用程序所需的 Clang C++ 编译器、Xcode IDE 和其他 Apple 开发工具。如果您是第一次安装 Xcode，或者刚刚安装了新版本，那么您需要同意可以进行命令行构建之前的许可证：

``` sudo xcodebuild -license accept ```

我们的 OS X 构建脚本使用 [Homebrew](https://brew.sh/) 包管理器来安装外部依赖项。如果您想从头开始，这里是卸载 [Homebrew](https://docs.brew.sh/FAQ#how-do-i-uninstall-homebrew) 的方法。

### Windows 先决条件

你需要为Windows版的Solidity构建安装以下依赖

| 软件 | 备注 |
| :----: | :----: | 
| Visual Studio 2019 Build Tools | C++ 编译器 |
| Visual Studio 2019(可选) | C++编译器和开发环境 |
| Boost(Version 1.77+) | C++库 |


如果您已经拥有一个 IDE，并且只需要编译器和库，则可以安装 Visual Studio 2019 Build Tools。

Visual Studio 2019 提供了 IDE 以及必要的编译器和库。因此，如果您没有 IDE 并且更喜欢开发 Solidity，Visual Studio 2019 可能是您轻松设置所有内容的选择。

以下是应安装在 Visual Studio 2019 Build Tools 或 Visual Studio 2019 中的组件列表：

- Visual Studio C++ 核心功能

- VC++ 2019 v141 工具集（x86、x64）

- Windows 通用 CRT SDK

- Windows 8.1 开发工具包

- C++/CLI 支持

我们有一个帮助脚本，您可以使用它来安装所有必需的外部依赖项：

```
scripts\install_deps.ps1

```

这会将 ```boost``` 和 ```cmake``` 安装到 ```deps``` 子目录。

### 克隆仓库

要克隆源代码，请执行以下命令：

```
git clone --recursive https://github.com/ethereum/solidity.git
cd solidity
```

如果你想帮助开发 Solidity，你应该 fork Solidity 并将你的个人 fork 添加为第二个远程：

```
git remote add personal git@github.com:[username]/solidity.git

```

<div style="background:#76afdb;color:#fff;padding:5px;font-weight:600">
⚠️ Note
</div>
<div style="background:#e9f2fa;color:#404040;padding:10px">
这种方法将导致一个预发布的构建，例如，在这种编译器产生的每个字节码中设置一个标志。如果您想重新构建一个已发布的 Solidity 编译器，那么请使用 github 发布页上的源压缩包。

[https://github.com/ethereum/solidity/releases/download/v0.X.Y/solidity_0.X.Y.tar.gz](https://github.com/ethereum/solidity/releases/download/v0.X.Y/solidity_0.X.Y.tar.gz)

（不是GitHub提供的“源代码”）。

</div>

<br/>

### 命令行构建

**确保在构建之前安装外部依赖项（见上文）。**

Solidity 项目使用 CMake 来配置构建。你可能想要安装 ccache 来加速重复构建。 CMake 会自动拾取它。在 Linux、macOS 和其他 Unices 上构建 Solidity 非常相似：

```
mkdir build
cd build
cmake .. && make
```

或者在 Linux 和 macOS 上更容易，你可以运行：

```
#note: this will install binaries solc and soltest at usr/local/bin
./scripts/build.sh
```

<div style="background:#e9b684;color:#fff;padding:5px;font-weight:600;">
⚠️ Waring
</div>
<div style="background:#fdeecf;color:#404040;padding:10px">
BSD 构建应该可以工作，但未经 Solidity 团队测试。
</div>

<br/>

Windows 用法

```
mkdir build
cd build
cmake -G "Visual Studio 16 2019" ..
```

如果您想使用 ```scripts\install_deps.ps1``` 安装的 boost 版本，您还需要将 ```-DBoost_DIR="deps\boost\lib\cmake\Boost-*```" 和 ```-DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded``` 作为参数传递给调用制作。

这应该会导致在该构建目录中创建 **solidity.sln**。双击该文件应该会导致 Visual Studio 启动。我们建议构建发布配置，但所有其他的都可以。

或者，您可以在命令行上为 Windows 构建，如下所示：

```
cmake --build . --config Release
```

## CMake 选项

如果您对可用的 CMake 选项感兴趣，请运行 ```cmake .. -LH```。

### SMT Solvers

Solidity 可以针对 SMT 求解器构建，如果在系统中找到它们，默认情况下会这样做。每个求解器都可以通过 cmake 选项禁用。

注意：在某些情况下，这也可能是构建失败的潜在解决方法。

在构建文件夹中，您可以禁用它们，因为默认情况下它们是启用的：

```
# disables only Z3 SMT Solver.
cmake .. -DUSE_Z3=OFF

# disables only CVC4 SMT Solver.
cmake .. -DUSE_CVC4=OFF

# disables both Z3 and CVC4
cmake .. -DUSE_CVC4=OFF -DUSE_Z3=OFF
```

## 详细的版本字符串

Solidity 版本字符串包含四个部分：

* 版本号

* 预发布标签，通常设置为 ```develop.YYYY.MM.DD``` 或 ```nightly.YYYY.MM.DD```

* 以 ```commit.GITHASH``` 格式提交

* 平台，它有任意数量的项目，包含有关平台和编译器的详细信息

如果有本地修改，提交将以 ```.mod``` 为后缀。

这些部分按照 SemVer 的要求进行组合，其中 Solidity 预发布标签等于 SemVer 预发布，Solidity 提交和平台组合构成了 SemVer 构建元数据。
发布示例：```0.4.8+commit.60cc1668.Emscripten.clang```。

预发布示例：```0.4.9-nightly.2017.1.17+commit.6ecb4aa3.Emscripten.clang```

## 关于版本控制的重要信息

发布后，补丁版本级别会发生变化，因为我们假设只有补丁级别发生变化。合并更改时，应根据 SemVer 和更改的严重性来提升版本。最后，发布总是使用当前每晚构建的版本，但没有预发布说明符。

Example:

1. The 0.4.0 release is made.

2. The nightly build has a version of 0.4.1 from now on.

3. Non-breaking changes are introduced –> no change in version.

4. A breaking change is introduced –> version is bumped to 0.5.0.

5. The 0.5.0 release is made.

此行为适用于[版本杂注](./Pragma.md)。