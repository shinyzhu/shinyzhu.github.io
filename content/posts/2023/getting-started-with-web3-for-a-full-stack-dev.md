---
title: "一个全栈开发者的 Web3 入门之旅"
description: "Web3 整体知识介绍和上手指南"
date: 2023-07-28T11:00:00+08:00
lastmod: 2023-07-28
tags: [web3]
featured_image: "/posts/2023/getting-started-with-web3-for-a-full-stack-dev/web3-design-banner.png"
categories: Blog
comment: true
---

## 一个全栈开发者的 Web3 入门之旅

### 内容概要

这篇文章是我在学习 Web3 的一些总结，主要包括了解 Web3 的整体蓝图、如何开始第一个 Web3 的代码，另外还会涉及到一些开发者社区和安全性相关的话题，以及怎样进行下一步的学习。如果你是一名开发者，熟悉全栈 Web 开发，希望你能通过阅读本文对 Web3 有一个更确定的认识。

### 关于作者

我是一名“全栈开发者”，为什么要这么称呼自己呢？

我想从我的第一份工作说起。那是还在大学就读，因为学习的是计算机专业，所以一直就想着能有用武之地，所幸当时正好有一家旅行社要做门户网站，我就跟另一个小伙伴去兼职了。我所负责的工作是前端设计和开发。那是2004年左右，你觉得会用什么样的技术来做前端呢？

![](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/xutour-web-2006.png)

我们现在用的前端技术 HTML、CSS 和 JavaScript 其实在那个时候就已经发展得不错了，但对于喜欢使用新技术的我来讲，更吸引我的是当时出现的 [Web 标准](https://www.w3.org/wiki/The_web_standards_model_-_HTML_CSS_and_JavaScript)，一个最大的特点是完全抛弃用 `table` 来布局，改成了 CSS 来控制页面元素的展示。今天你也知道了，这已经成为了前端的标准，CSS 用来控制页面展示，HTML 用来确定语义化的结构，JS 用来控制交互和行为。

当时这么做还带来了其他优势。我也在那个时候做了 SEO（搜索引擎优化）的工作。网页做好了就需要去推广，而使用 Web 标准制作的网页在搜索引擎上是有优先展示的，Google 给我们很多个网页的 PageRank 评分超过了 5。从而使得很多关键字的搜索结果都是我们旅行社的网页，所以在商业模式上也让这家旅行社从地推模式转向了互联网模式，第二年超过 80% 的收入都来自这个门户网站。

再往后的工作和学习经历，我开始接触到服务端的应用开发，实现业务逻辑和性能要求等等。再后来开始接受数据库和数据仓库类项目，让我对数据开始着迷，甚至还做了移动端的数据可视化  App。以及自己喜欢学习和应用新技术，也在技术社区里一直活跃。这也引导我后面的职业发生了变化。

### 什么是“全栈”

先回到“全栈”。我们知道开发者的工作包含各种各样的内容，在不同的公司也有各种不同的称呼。我们从一个典型的软件应用的分层设计来看这个问题。

![](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/mf-layered.png)

[Martin Fowler](https://martinfowler.com/aboutMe.html) 是我崇拜的人之一，上图来自他的[博客文章](https://martinfowler.com/bliki/PresentationDomainDataLayering.html)，这样我们对分层就有一个直观的印象。他在软件应用架构领域有很多著作。其中之一是《[企业应用架构模式](https://martinfowler.com/books/eaa.html)》，第一章就介绍了“分层”设计。

![mf-eaa-1](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/mf-eaa-1.png)

所以，简单来讲，全栈就是指从分层设计来看覆盖展示层、领域层和数据层的整个“栈”，全栈开发者就是指能够在各层进行设计和开发的程序员。

我们常说的“前端”是指 Web 端。其实还有很重量级的（Client）客户端，它需要在用户的设备上安装该应用的独立软件才能够使用。我们就以常见的 Web 端来展开，这也是本文的主题之一。

### Web3 是什么

Web 标准的应用极大提高了用户的体验，同时出现的还有很多创新的应用，比如社交网络，用户能参与到网站的内容创作，即 UGC（用户创作的内容）。这种用户可以参与网站内容创作的模式被称为 Web 2.0。

那 Web3 是如何出现的呢？如果你也用过 BT 下载，那你就已经是去中心化应用的用户了。

![](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/web-1-2-3.png)

我认为 Web3 还是一个比较模糊的概念，很多内容还在不断变化，但目前达成共识的一种定义是基于去中心化的区块链作为数据存储层，利用多种协议和共识机制作为业务逻辑层，以及在这些底层之上的各种应用，比如比特币、NFT、DApps、IPFS，以及智能合约、分布式金融 DeFi、游戏金融GameFi等。

是不是对应到我们熟悉的分层，就更容易理解 Web3 的技术栈了呢？对应到 Web3 的分层，也有对应的术语，比如 Layer 1，Layer 2，Layer 3。另外还有很多很多新的术语，都代表着某一个领域里新的和更好的一种愿景。

那么，为什么我要关注和使用 Web3 呢？

首先是因为 Web3 是新技术体系，我个人非常感兴趣，但是跟比特币需要投入资金去交易不同，Web3 是技术向的，对于开发者非常友好，也是我们能够参与进去的。

其次是从目前的应用来看，已经诞生了一些非常有趣的场景，比如 NFT（暂且抛开交易和炒作的影响）用在开发者社区的激励也是非常有意思，比如[我的 Azure Learner 徽章](https://enjinx.io/eth/asset/608000000000050e/3414)，就是一个 NFT。

![](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/azure-learner.png)

最后对于新的技术潮流，如果你跟我一样想在以后能做一些应用，最好的方式就是真的进入这个领域并上手使用它们。

另外一点就是因为目前 Web3 的整体版图已经发展到具有一定的成熟度，对于常规技术采用者来讲，现在就是一个很好的切入时机。

### 开始使用 Web3 的技术

希望通过以上部份的阅读和了解，你现在已经对 Web3 有了一个大概的整体认知，接下来我将带你真正进入 Web3 的开发。

跟我们做全栈开发类似，你也要根据你要做的项目来确定从哪里开始。

![](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/the-web3-stack.png)

#### Layer 1 和 Layer 2 应用开发

从现在开始我们更多地使用 Web3 领域的词汇。哈哈哈。

Layer 1 通常指区块链，Layer 2 通常指区块链的扩展层。如果你要从头开始做一个应用，对于数据层你有很多选择，在 Web3 领域也不例外。但是会变成这样一个问题：

我要选择哪一个区块链？

是直接使用以太坊进行应用开发，还是需要自己发行区块链？

对于自己开发区块链，我在多年前模仿实现了一个简单的应用。[源代码在这里](https://github.com/shinyzhu/simpleblockchain)。

```csharp
using System;

namespace SimpleBlockchainApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Simple Blockchain now starting...");

            var blockchain = new Blockchain();
            blockchain.CreateTransaction(new Transaction("user1", "user2", 100));
            blockchain.CreateTransaction(new Transaction("user2", "user1", 50));

            System.Console.WriteLine("Starting the miner...");
            blockchain.MinePendingTransactions("miner");
            System.Console.WriteLine($"Balance of the miner is {blockchain.GetBalanceOfAddress("miner")}");

            System.Console.WriteLine("Starting the miner...again");
            blockchain.MinePendingTransactions("miner");
            System.Console.WriteLine($"Balance of the miner is {blockchain.GetBalanceOfAddress("miner")}");
        }
    }
}
```

当然要完整实现一个产品级别的区块链并不容易，但也是非常有趣的一个新领域。

在Layer 1 中，可以选择使用以太坊、Polkadot等平台进行开发。以太坊是目前最为流行的 Web3 平台之一，具有最广泛的社区和最丰富的生态系统。Polkadot 则是一个新兴的多链平台，可以实现跨链互操作和可扩展性。针对 Layer 2 可以选择使用 Rollups 和 Sidechains 等解决方案进行开发。Rollups 是一种将智能合约放置在 Layer 2 上的方案，可以提高交易处理速度和降低交易费用。Sidechains 是一种在区块链平台之外创建的区块链，可以实现更高的性能和可扩展性。

#### Layer 3+ 应用开发

Layer 3 通常是指应用层了，有非常多的创新发生在这里，也出现了更抽象的上层，但我们这里简单划分为应用层。因为底层的去中心化属性，所以Web3 的应用大多数成为 DApps（Decentralized Applications，去中心化应用）。

不要被 DApps 的称呼吓到，它其实是一个种类的称呼。具体来讲还有很多不同的具体应用和形式。比如比特币等加密货币就是一个典型应用，诞生了钱包、交易所等应用。也诞生了分类包括 DeFi （Decentralized Finance，去中心化金融）和 GameFi（游戏金融）。

DApps 的核心是智能合约（Smart Contract），它是一段存储在底层存储（区块链）上的可执行的代码。以下就是一段 `Hello World` 的 Solidity 代码，它可以运行在以太坊虚拟机 EVM 上。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
contract HelloWeb3{
    string public hello = "Hello Web3 from Shiny!";
}
```

可以使用 [Remix IDE](https://remix.ethereum.org/) 进行合约开发，以下是运行效果。可以看到运行后的变量 `hello` 的值显示为代码里定义的 `Hello Web3 from Shiny!`。

![remix-solidity-hello](/posts/2023/getting-started-with-web3-for-a-full-stack-dev/remix-solidity-hello.png)

从开发的角度来看，DApps 为企业提供了无需管理和维护后端基础设施的优势。智能合约存储在区块链上并自主运行。部署 DApps 的企业通常还需要部署和维护用户界面，这本就是应用的上层交互层，使用中间服务对后端的智能合约进行 API 查询。DApps 是可靠的，因为它们在庞大的点对点网络上运行，而中心化应用程序如果其支持基础设施出现故障，就会出现故障。

从应用角度看，还有很多其他的不同应用，比如 IPFS 用于内容存储和发布等等，这里有一个列表，可以帮助我们更好地了解 Web3 开发：

1. 以太坊上的去中心化交易所：Uniswap
2. 使用 IPFS 协议的 P2P 即时通讯软件：Berty
3. V2EX 站长 Livid 基于 IPFS 和 ENS 的开源内容分发系统：Planet
4. 建立在 IPFS 与 ENS 的去中心化 Zoom：Huddle
5. 建立在 IPFS 与 Arweave 上的去中心化视频网站：Lenstube
6. 建立在 Arweave 上的永久存储不会像传统网盘一样丢失数据的去中心化云盘：Ardrive
7. 建立在 Arweave 上的去中心化博客网站：Mirror

另外，很多公有云也提供了区块链和 Web3 基础服务，比如 [Azure](https://azure.microsoft.com/en-us/solutions/web3/) 等。

### 实现安全性

为什么一入门就要了解安全性呢？因为 Web3 天生的特点是去中心化和金融属性，虽然 Web3 的基本原则使其在某些方面比 Web 2.0 更安全，但与任何技术一样，它也会带来一定的安全风险。一些安全漏洞来自 Web3 和 Web 2.0 架构的交互方式；其他一些是区块链和 IPFS 等协议如何运作所固有的。 Web3 对网络共识的依赖可能会使修补这些缺陷和其他缺陷成为一个缓慢的过程。在 Web3 开发中，安全性是非常重要的问题，尤其是涉及到数字资产和智能合约，一旦出现安全漏洞或攻击，可能会导致重大损失。

因此，开发者需要在开发过程中重视安全性，并采取各种措施来保证应用的安全性。Web3 的安全风险有哪些呢？

1. API 查询缺乏加密和验证
2. 智能合约黑客攻击
3. 去中心化数据存储的隐私问题
4. 账户被盗和手机钱包被盗
5. 协议和桥接攻击
6. 漏洞和安全修复进展缓慢

在项目开始阶段就考虑实现安全相关的需求，可以帮助确保项目的安全性和可靠性。安全领域有许多行业标准，例如：ISO 27001、ISO 27002、PCI DSS、SOC 2、NIST SP 800-53等。这些标准提供了安全审计的框架和指南，以帮助组织评估其信息系统的安全性，并确定是否存在任何安全漏洞。通常来说，软件系统的安全需求包括这些方面：使用安全开发最佳实践、进行代码审查、使用安全工具和框架、进行安全测试和漏洞扫描等 。

我们还可以考虑使用一些工具和框架来进行安全审计，例如：Semgrep、OWASP ZAP、Nessus等。此外，还可以考虑聘请专业的安全审计公司来帮助进行安全审计。其中，第三方审计是一种非常重要的措施。开发者可以请专业的审计公司对应用进行审计，发现并修复潜在的安全漏洞和问题。

### 下一步

Web3 技术的发展非常迅速，每天都有新的技术和应用出现。作为一名有全栈经验和知识的开发者，需要通过不断学习和实践，不断提高自己的技术水平，并且参与到 Web3 生态系统的建设和发展中。

Web3 大多数技术都是开源的，有着非常活跃的技术社区，除了在官方的社区平台，也可以在 GitHub 和 StackOverflow 等平台上找到共同爱好的开发者进行交流学习。

阅读到此，本文也差不多可以告一段落了。希望你通过本文可以更清楚的了解 Web3 技术生态和有兴趣马上开始进入 Web3 的具体项目和具体代码开发中。

#### 参考资料

1. [What is Web3?](https://www.certik.com/resources/blog/Web3) by CertiK
2. [Introduction to Web3](https://ethereum.org/en/web3/) by Ethereum
3. [Defining the web3 stack](https://edgeandnode.com/blog/defining-the-web3-stack/)
4. [What is Web3 Security?](https://www.certik.com/resources/blog/61IYiqqASgw9kA5BsmaqKa-what-is-web3-security) by CertiK
5. [Understanding Web3 and its security implications](https://www.cloudflare.com/learning/insights-web3-security/) by Cloudflare