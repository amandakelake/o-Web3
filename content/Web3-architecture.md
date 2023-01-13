# Web3技术架构

> 本文基于 [The Architecture of a Web 3.0 application](https://www.preethikasireddy.com/post/the-architecture-of-a-web-3-0-application) 有一定的精简和转译


## 一、Web2网站架构

对于一个完整的Web2应用，即使它很简单，也有几个需要考虑的地方

* 数据库：存储用户信息、业务数据的地方
* 后端服务（Node/Java/Python/Go）：定义业务逻辑
* 前端服务：定义网站/应用的UI、交互逻辑

Web2技术架构的高级抽象如下

> 这里不讨论更高级的分布式架构，仅仅是高度抽象

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131727673.png)

## 二、Web3 基础架构
Web3最大的区别在于去中心化，不需要集中式的数据库或者Web服务器，利用区块链分发应用

区块链充当状态机，后端代码通过智能合约实现，并部署到共享状态机（区块链网络）

架构如下

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131727853.jpg)

## 三、前端与智能合约通信

前端应用层的展示跟Web2几乎一样，最大的不同在于与智能合约通信

前端应用想要和区块链上的数据和代码交互时，需要与其中一个节点进行交互。任何节点都可以广播交易，矿工帮助执行这笔交易并将结果状态广播到整个区块链网络

广播交易有两种方式
* 自行搭建一个运行以太坊的节点
* 使用第三方服务提供的节点，比如 Quicknode、Alchemy、Infura

通信时连接的节点被称为 Provider

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131727155.jpg)

一旦通过 Provider 连接到区块链，就可以读取存储在区块链上的状态

如果想写入状态，在提交交易到区块链之前，还需要使用私钥为交易签名，最常用的签名工具就是 Metamask

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131728861.jpg)

Metamask 是一个工具，使应用轻松地处理密钥管理和交易签名。它非常简单：Metamask 在浏览器中存储用户的私钥，当前端需要用户对交易进行签名时，它会调用 Metamask。

Metamask 还提供与区块链的连接（作为一个 Provider），内置连接了 Infura 提供的节点用于签名交易

Metamask 既是 Provider，也是 Signer


## 四、区块链中的存储

在区块链上存储虽然取用方便，但却非常昂贵。用户每次需要添加新的数据到以太坊上时都需要支付Gas，用户体验也不好

常用的解决方案：使用去中心化的链下存储方案，如 IPFS 或 Swarm

IPFS 是一个支持存取数据的分布式文件系统。因此，IPFS 系统不是将数据存储在中心化数据库中，而是将数据分发并存储在一个 P2P 网络中。PFS 还有一个叫做「Filecoin」的激励层，它通过激励机制使世界各地的节点来存储和检索这些数据

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131728983.webp)

前端代码大部分是静态资源，在Web2是存在中心化的云服务，比如AWS的S3

在Web3架构里，DApp也应该将前端资源存在 IPFS/Swarm 这种去中心化的存储解决方案上

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131728661.jpg)

## 五、区块链上的查询

从区块链上的智能合约中读取数据，可以通过智能合约事件(Events)，使用 Web3.js 库查询和监听智能合约事件，局限在于智能合约的事件代码如果有更新需要重新部署

The Graph 是一个链下的索引方案，简化了以太坊上的数据查询

借助 The Graph，你可以定义哪些智能合约需要索引、哪些事件和函数调用需要监听，可以规定如何将传入的事件转化为前端逻辑（或是使用 API 的程序）可处理的实体

通过索引区块链数据，The Graph 实现了链上数据查询的低延迟

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131728063.jpg)

## 六、扩容

以太坊暂时不具备可扩容性，由于以太坊的高额 gas 费和饱和的区块，在上面部署 DApp 会导致非常糟糕的用户体验

目前主流的扩容方案有Polygon（侧链），在链下执行交易

![](https://raw.githubusercontent.com/amandakelake/picgo-images/master/images/202301131728499.jpg)
