# 简介
[Layer0白皮书](https://layerzero.network/pdf/LayerZero_Whitepaper_Release.pdf)

区块链技术在这几年迅速兴起和发展，针对最初阶段暴露出的问题提出了各种解决方案。一直以来，链与链之间的孤立关系都是被重点关注的问题之一，为此也出现了许多方案去解决跨链资产交换。
Layer0是第一个trustless omichain interoperability protocal, 本质上提供一种协议实现跨链应用。开发者可以不通过第三者(第三条链或可信托管人)而直接完成跨链。

# 实现
## 有效交易 `Valid Delivery`
有效交易通过以下两点保证：
1. 每个通过网络发送的消息m都和发送方链上的交易t相联系
2. 当且仅当相关交易t是有效的而且已经在发送方链上提交时，消息m才会交付给接收方

## LayerZero组件

### LayerZero Endpoints
每条链都有一个layerzero的端点，它由一系列链上的智能合约实现。端点允许用户使用layerzero协议的后端发送消息，并且保证有效传递。

## Oracle
Oracle提供一种机制，从链上读取区块头并将其发送到另一条链。

## Relayer
中继器(Relayer)提供另一种链外服务，它获取指定交易的证明。任何使用LayerZero协议发送的信息，oracle和relayer必须相互独立。


# 实现过程
1. 用户在A链上做交易T，假设交易标识符为t，交易T包含的一个步骤是，在T的条件下，信息在`LayerZero`传输有效。
2. 应用向`LayerZero Communicator`发送请求，标识符、B链地址、payload、relayer参数
3. `LayerZero Communicator`向 `Validator`发送一个数据包
4.  `LayerZero Validator`将数据包发给中继器Relayer
5.  网络把合约地址和当前交易的区块ID发给Oracle，`Oracle`将会获取该区块的区块头并发送到B链。
6. `Oracle`读取block header
7. `Relayer`读取交易T相关的交易证明 proof(t), ** 6和7互相独立进行**
8. `Oracle`确认header相关的区块在A链被确认，然后将header发给B链的网络
9.  B链网络把block header发给`Validator`
10. `Validator`转发给中继器
11. 中继器发送与当前区块匹配的任何Packet(dst, payload), t, proof(t) tuple。
12. `Validator`使用收到的交易证明和网络储存的block header验证相关的交易T是否有效和被提交。
13. 通信器向应用程序B发送数据包