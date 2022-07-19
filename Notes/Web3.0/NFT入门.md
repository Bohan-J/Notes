今年被称为 NFT 的“元年”，各个公司(可以搜搜 bytechain, 是字节的区块链产品)都开始制作、发放自己的数字藏品；加密艺术家Beeple的数字作品“First 5000 Days”在佳士得单一拍品网上以6900万美元价格成交；Uniswap一双袜子卖16万美元。这一切让人觉得有点不可思议，那么数字藏品究竟是什么、从何而来、如何产生，又将去向何处呢？

# 背景

## 名词解释

先来一段wikipedia上的简介

> **非同质化代币**（英语：Non-Fungible Token，简称：**NFT**），是一种被称为[区块链](https://zh.m.wikipedia.org/wiki/%E5%8C%BA%E5%9D%97%E9%93%BE)数位账本上的数据单位，每个代币可以代表一个独特的[数码资料](https://zh.m.wikipedia.org/wiki/%E6%95%B8%E7%A2%BC%E8%B3%87%E6%96%99)，作为虚拟商品所有权的电子认证或凭证。由于其不能互换的特性，非同质化代币可以代表数位资产，如[画作](https://zh.m.wikipedia.org/wiki/%E7%BB%98%E7%94%BB)、[艺术品](https://zh.m.wikipedia.org/wiki/%E8%97%9D%E8%A1%93%E5%93%81)、[声音](https://zh.m.wikipedia.org/wiki/%E8%81%B2%E9%9F%B3)、[影片](https://zh.m.wikipedia.org/wiki/%E8%A7%86%E9%A2%91)、[游戏](https://zh.m.wikipedia.org/wiki/%E6%B8%B8%E6%88%8F)中的项目或其他形式的创意作品。虽然作品本身是可以无限复制的，但这些代表它们的代币在其底层区块链上能被完整追踪，故能为买家提供所有权证明。

非同质化：指不能够进行互换，在我们通用的货币概念中，A的100美金和B的100美金是一模一样的，他们可以直接被互换，也可以拆分成两个50美金，这即是同质化。

对于NFT一段个人的理解：本质上来说每个NFT是存在链上的一段数据，由于其唯一性、不可分割性，我们可以把它理解成一种全新的、数字化权利形式。最终目标号称是将信息网络（当前的网络，只有信息，但没有所有权，没有价值）向价值网络（所有的信息都有所有权，因此有了价值，信息可以复制，但价值只能转移）转变，基础设施就是NFT。

## 起源

NFT的概念是在2017年由加密猫Cryptokitties的创始人提出的，当时市场上的以太坊token都是基于ERC20协议开发出的同质化Token，而Cryptokitties将自己游戏中的每只猫基于ERC721协议（也就是现在的NTF开发标准协议)发布上链，随着加密猫的火爆，NFT这个概念进入了Web3领域的视线。

NFT经过2018-2019年的回归建设，从2020年开始了爆发，从一开始作为风险投资产品、艺术品以及明星个人周边发售，到2021年开始与GameFi一起快速发展。 从2021年2月开始，NFT的总市值暴涨超过2000%，其搜索量、交易人数、总交易量都呈现了爆炸式增长。

# 技术实现

这里我用个人觉得比较浅显但清晰易懂的方式去解释NFT，因为本文面向的对象(包括作者自己)主要是对Web3零经验或仅仅知道皮毛的同学，希望能够对其有一个基本的概念。

## 智能协议

本质上来说，链上的token都是存在链中某个位置的一段数据，NFT也是如此。在web3开发中，是没有后端这个概念的，DApp的页面通过钱包和web3相关的js库可以直接读写链上的数据。但是显然，我们需要某些程序语言去描述、约定一个NFT collection的相关属性，也需要暴露出对应的方法让外界去与链上数据进行交互，这里就引入了智能合约(smart contract)这个概念。

智能合约是一段发布在链上的代码，一条区块链通过触发smart contract的不同事件来改变区块上的数据。这段代码一经发布无法修改，一个NFT collection的所有属性和操作都是由这段代码规范的。为了保证数字藏品的流通性和技术实现的一致性，目前以太坊上均采用ERC721或基于ERC721协议进行拓展的协议发售NFT。

别的不多说，先来看下以太坊对于ERC721的接口暴露协议规定

[EIP-721: Non-Fungible Token Standard](https://eips.ethereum.org/EIPS/eip-721)

```JavaScript
pragma solidity ^0.4.20;
interface ERC721  {
    event Transfer(address indexed _from, address indexed _to, uint256 indexed _tokenId);/// @dev This emits when the approved address for an NFT is changed or
    event Approval(address indexed _owner, address indexed _approved, uint256 indexed _tokenId);/// @dev This emits when an operator is enabled or disabled for an owner.
    event ApprovalForAll(address indexed _owner, address indexed _operator, bool _approved);/// @notice Count all NFTs assigned to an owner
    function balanceOf(address _owner) external view returns (uint256);/// @notice Find the owner of an NFT
    function ownerOf(uint256 _tokenId) external view returns (address);/// @notice Transfers the ownership of an NFT from one address to another address
    function safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes data) external payable;/// @notice Transfers the ownership of an NFT from one address to another address
    function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable;/// @notice Transfer ownership of an NFT -- THE CALLER IS RESPONSIBLE
    function transferFrom(address _from, address _to, uint256 _tokenId) external payable;/// @notice Change or reaffirm the approved address for an NFT
    function approve(address _approved, uint256 _tokenId) external payable;/// @notice Enable or disable approval for a third party ("operator") to manage
    function setApprovalForAll(address _operator, bool _approved) external;/// @notice Get the approved address for a single NFT
    function getApproved(uint256 _tokenId) external view returns (address);/// @notice Query if an address is an authorized operator for another address
    function isApprovedForAll(address _owner, address _operator) external view returns (bool);
}
```

以太坊规定任何基于ERC721协议部署的smart contract都要继承以上的interface，与此同时我们还需要实现一个mint函数，即‘挖矿’函数，用来产生一个NFT。

我个人看后的第一个感觉：这不就是实现针对一个Class类型的OOP编程？给定一个对象需要的能力，我们只需要把对应的具体内容填充完整(即函数逻辑)

实际上发布NFT的时候我们可以直接使用openzeppelin提供的协议模版定制化自己的内容。

[ERC 721 - OpenZeppelin Docs](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721)

> OpenZeppelin Contracts 是一个用于安全智能合约开发的库。 它提供了 ERC20 和ERC721 等标准的实现，您可以按需部署或扩展以满足您的需求，还提供Solidity组件来构建自定义合同和更复杂的分散系统。

## 存储

NFT是一种数字资产，那么如何把每个NFT和拥有者进行关联呢？答案很简单，即发布NFT的smart contract会规定几个map来存储nft的所有关系，我觉得对于入门级学生来说，可以直接理解成链上有key-value对应的map来注明一个nft属于谁。当我们进行nft交易的时候，只需要更改此处的key-value关系就能实现NFT的transfer。

需要注意的一点是，NFT常常与艺术作品相关关联，虽然NFT本身存在链上，但是大文件不行，因此NFT相关的图片、视频通常是在链下存储(ex. 分布式文件系统)，然后在链上存储这些文件的hash值，这导致NFT没有实现完全的去中心化，对资产持有者来说是存在很大隐患的。

# NFT的应用分布

下图是对web3领域的一个简单分层，NFT项目目前主要集中在数字收藏品、游戏资产和虚拟世界中

![](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2JlYzQ4NjI4ZDgzMWE3ODlhMzlhYWY5NjczMDQxZDVfeHhCM0FwSVZSWGduMFFtTG1ib2JmcXZzdVVQZzJKRUJfVG9rZW46Ym94Y25vMEpTVURWeGtiM29kUGMxelBzTFRmXzE2NTgxNTAxODA6MTY1ODE1Mzc4MF9WNA)

## 数字收藏品

数字收藏品往往是有特定文化印记和艺术美感的多媒体内容，像是头像、明星周边等都在其列。比较大的代表项目包括 Cryptopunks、bored ape、Phanta Bear等，其中总池最高的当属Cryptopunks

https://www.larvalabs.com/cryptopunks

2017年以太坊刚刚开始快速发展，两个之前甚至不在加密货币圈的开发者带着一万个像素头像进入该领域，并开发出了世界上第一个NFT项目——CryptoPunks。他们基于ERC20的标准进行了魔改，然后将这些头像搬到了以太坊上。

截止7.18日，目前1 ETH = 1361 USD

cryptopunks 最高成交 124457ETH * 1400 USD ≈ 1.7亿美金

目前cryptopunks collection的总价值在973kETH ≈ 132亿美金左右
![[Pasted image 20220719124745.png]]
Bored Ape
![[Pasted image 20220719124901.png]]
![](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=NTk0ZTk4OGZkYzNlZGI0MTJhMGQwNWI0OWFmY2RlYzdfUUYxMW1pMktHdExzOUlIdnRydWpaZG55MXFxRWhxZGJfVG9rZW46Ym94Y25aQk1vWG9xVGw1TmpNRDRINVFDdHVHXzE2NTgxNTAxODA6MTY1ODE1Mzc4MF9WNA)

## 游戏道具

GameFi的出现和发展大大促进了NFT市场的交易，游戏道具NFT更强调用途，其中最成功的几款产品包括
-   Cryptokitties
![[Pasted image 20220719124954.png]]
最早提出NFT概念的产品，玩家可以养猫并且给猫繁殖

- Axie infinity：Play to earn
![[Pasted image 20220719125026.png]]
这是一款开放式的回合制策略类数字宠物游戏，包括战斗、繁殖、地块和交易市场四个系统，小精灵即是矿机

-   STEPN：Move to earn
![[Pasted image 20220719125455.png]]
通过移动获取代币的健身应用程序，搭配NFT运动鞋的用户可以在户外活动来赚取代币，跑鞋是矿机


## 虚拟世界

作为虚拟世界的资产证明，比较大的项目有Decentraland，用户可以在里面拥有自己的土地并且进行建设（这个很像几个月前爆炒的VR土地）
![[Pasted image 20220719125346.png]]

# 价值判断

[推荐 | 读懂NFT：全面解析NFT发展简史、价值及未来](https://zhuanlan.zhihu.com/p/442753831)

上文提出了推动NFT价值的7个特征，我会加入一些个人的理解在其中。

## 七个判断纬度

### 1.链上安全 
在讨论资产的时候，安全性一定是被首要关注的。NFT作为挂载在链上的数字资产，如果说链上数据不安全，那相当于这部分资产时刻处在会丢失的状态当中。实际上几乎所有公链都可以发布NFT，但是为什么以太坊是当前NFT网络的统治者？就是因为它是目前运行的最安全的智能合约平台，它的底层建设有保证，并且在可预见的未来中可以保持主导地位。

### 2. 上链
就像是上面提到的储存问题一样，NFT资产如果有一部分是依赖于链下的，那如果储存该信息的中心化主机/服务器停止运行，那么拥有者可能在几年之后拥有一个空白的NFT。因此，NFT在链上的部分越多，他越可以证明自己，并且随时能够证明自己。

### 3. 年龄
就像是我们收藏文物一样，年份越早的文物往往更有价值，那么NFT也是一样。如果内容类似，那么铸造于2017年的NFT一定比2025年的NFT更具有收藏价值和升值潜力。

### 4. 创作者和社区
NFT的发布者都是可被追溯的，如果一个没有任何背景的无名之辈去Opensea上发布（例如我今天去发布了一组NFT), 显然它们的价值是很低的，而如果某个知名艺术家/开发者发布一组NFT，当然会有更多的人认同它的价值。

### 5. 稀缺性
物以稀为贵，与许多商品的饥饿营销同理，1 of 10000当然比 1 of 10更珍贵

### 6. 施放速度
如果让你以同价格选购NFT，你会选择每年发布一个还是每年发布10000个的collection？

### 7. 丰富性
相比仅一张图片，带有音频或视觉特效的NFT具有更多潜在价值。

## 个人理解

我本人对于金融和经济方面其实也没有很深入的理解，只从NFT新入门程序员的角度谈谈看法。

个人认为，当前阶段NFT的价值其实主要来自于共识，即整个Web3社区对一类事物价值的共同认知和认可。许多人在质疑NFT市场是否是一个巨大的泡沫，其实这个问题没有人可以回答，但是可以看到的一件事情是，NFT市场的总规模在持续变大、发售藏品的类型和功能在持续多元化。

同时，NFT其实是整个Web3.0发展过程中衍生出的一个领域，与其怀疑NFT是否是泡沫，不如说怀疑整个区块链加密货币市场都是泡沫。但是至少从过去几年来看，web3正处于明显的上升势头，其热度、应用领域、资本注入量、开发者社区都在不断增加/发展，而饱受质疑的加密货币经过几次起伏之后，最大规模的几种货币价格仍然坚挺。

与此同时，不可否认的是目前绝大多数人购买NFT都不是为了其艺术价值，而是为了对应藏品潜在的升值潜力，是一种投机行为。但是在所有赛道的早期，或者说在行业发展的任何一个环节，投资的主要目的一定是追求未来的价值提升，更多资本的进入代表有更多的人认为这个行业是仍有发展潜力的。

抛开资本，单纯从技术和创作理念上来说，web2发展到今天，大家都意识到数据控制权和所有权的重要性。在web3的观念中，用户自己的身份、数据都是由自己掌控的，数据可以保证持久存在，且不会被篡改或隐藏，这实际上是一场自下而上、打破平台垄断的革命。它符合绝大多数互联网使用者的利益，那么随着参与革命的人越来越多，也许真的能够打破web2的模式。

# 从零到一发布一款NFT （In the future）

这部分计划新起一个文档继续来写，会用[[Solidity]]基于openzeppelin提供的ERC721协议进行开发，前端应该是react+ether.js。完成后会把链接贴过来～