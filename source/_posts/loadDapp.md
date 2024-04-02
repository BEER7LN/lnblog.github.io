---
title: 闪贷Dapp的调研及实现
date: 2022-07-20
tags: 区块链
---

# 前置知识

- 区块链应用Dapp
  
  - 概念：去中心化应用（Decentralized Application, DApp）为建构于区块链上的应用程序，也被称之为分散式应用。
  
  - 架构：传统网站：前端→API→数据库。
    
    DApp类似于传统的Web应用程序，前端使用完全相同的技术来呈现页面，它包含一个与区块链通信的“wallet”，管理加密密钥和区块链地址。公钥基础结构用于用户标识和身份验证。与连接数据库的API不同，wallet so - ware触发了智能合约的活动，该智能合约与区块链交互：
    
    Web3.0网站：前端（包括wallet）→智能合约→区块链。
    
    - 智能合约：通常指代运行在 EVM 兼容网络中的 Solidity 或其他合约语言代码，他们负责与用户交易我们发行的资产并储存 DApp 的链上状态。
    - DApp：整合合约接口以及其他功能的应用程序界面，目前，它们大部分是 Web App，你可以用流行的框架例如 React/Vue 来进行编写。
    - Provider/Signer: 这是一个 DApp 架构中特殊的角色，它负责与区块链进行通信，并进行合约的读/写操作。Metamask 是一个流行的 InjectProvider（Web3Provider）你也可以使用其他 JSON-RPC Provider 与区块链进行通信。
    - Relay: 这个角色隐藏在 Provider/Signer 之后，是真正负责我们与区块链的某一个节点同步状态的服务器集群，它保存了所有账本（全节点）它通常是 Infura、Alchemy、Quicknode、Moralis 或者 Pocket 提供的服务。
    - 服务端（可选）：大部分 DApp 仍然有他们的服务端逻辑，这意味着，你需要自己搭建服务环境，或使用流行的 BasS/FaaS 服务，你可以使用深度整合区块链的 Moralis 来完成服务端的开发，也可以使用成熟的 Firebase 体系。当然，你也可以挑战完全不依赖服务端的方式来构建 DApp，就像 Uniswap 所做的那样。
  
  - 开发步骤：
    
    - 准备：
      - 公链：公链节点rpc接口，公链钱包，公链gas对应的代币
      - 学习公链接口的访问形式
      - 准备dapp浏览器或钱包插件
    - 开发：
      - 开发智能合约（ETH、bsc、tron合约，solidity语言）
      - 部署智能合约（在测试网申请测试币后部署智能合约，remix工具）
      - 开发dapp前端（vue框架，涉及钱包连接、合约调用、数据查询）
      - 后端开发
    - 技术:
      - web3，web3.js
      - vue框架
      - metamask插件
      - truffle框架简化Dapp开发流程

- 闪电贷flash loans
  
  - 概念：用户在借到款之后，可以利用借到的资产进行其他操作，然后在交易结束的时候，用户只要把借到的款项及手续费及时归还就可以，否则该笔交易就会回滚，就像什么也没有发生过一样。
  - DeFi 项目：
    - Aave是最早将闪电贷进行实际应用的DeFi 协议，主要原理是通过调用 Aave 中 lendingPool 合约中的 flashLoan() 函数实现借贷，通过executeOperation() 函数实现具体的业务逻辑。
    - dYdX 是一个针对专业交易者的去中心化交易所，本身并没有闪电贷功能，但是可以通过对 SoloMargin 合约执行一系列操作来实现类似闪电贷功能。其主要原理是通过继承 DydxFlashloanBase 合约编写initiateFlashLoan() 回调 callFunction() 实现借贷、套利、还款等操作。

# 业务流程与重要函数

说明：参考Aave(基于ETH公链)的业务流程与源码

![alt text](loanprocess.png)

flashLoan 函数实现借款功能，executeOperation函数实现还款功能

- 用户调用flashLoan 函数。传入参数：用户借款地址、外借资产地址、借款金额、债务模式等

- flashLoan 函数进行借款金额判断。验证外借资产地址余额是否小于借款金额，如果是就回滚初始状态，不是继续后续操作

- flashLoan 函数进行手续费判断。计算手续费(带宽和能量)，验证手续费是否大于 0，是则继续后续操作，否则回滚初始状态

- flashLoan 函数向用户借款地址转账。将用户借款地址定义为可接受转账地址，并从外借资产地址向用户借款地址转账借款金额

- executeOperation 函数执行附带调用信息，并进行还款操作。在 flashLoan 函数进行有效转账后，executeOperation 函数将被触发并接受来自 flashLoan 函数的传参
- executeOperation 函数将验证接收到的金额是否正确，不正确则回滚初始状态
- 借款用户系列操作xxx
- executeOperation 函数偿还资金。调用函数交易，偿还资金，还款金额= 借款金额 + 手续费
- flashLoan 函数进行转账前与还款后余额比较。还款完成后，进行转账前后余额比较，如果转账前余额+手续费与还款后余额不相等回滚初始状态，相等则继续后续操作
- 还款完成，更新闪电贷进程，触发闪电贷交易完成事件

# # 参考资料

完整操作步骤及源码：[Creating a Flash Loan using Aave](https://www.alchemy.com/overviews/creating-a-flash-loan-using-aave)
