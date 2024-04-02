---
title: "数学辅导新篇章：大语言模型的智慧助力"
description: "I tried several LLMs to solve a math problem for my kid"
date: 2024-04-02T16:11:00+08:00
lastmod: 2024-04-02
tags: [llm, math]
categories: Blog
featured_image: "/posts/2024/solving-a-math-problem-by-llm/solve-math-by-llm.jpg"
images: ["/posts/2024/solving-a-math-problem-by-llm/og-solve-math-by-llm.png"]
---

## 简介

作为家长，我们总是希望为孩子提供最好的教育支持，特别是在数学这一关键学科上。数学不仅是孩子们认识世界的重要工具，也是培养逻辑思维和解决问题能力的基础。然而，面对复杂的数学问题，即使是最有爱心和耐心的家长，有时也会感到力不从心。

在这样的背景下，大语言模型的出现，为我们提供了一种全新的教育辅导方式。这些智能工具不仅能够解答复杂的数学问题，还能够以易于理解的方式向孩子们解释概念和解题步骤，极大地减轻了家长的负担。

本文将从家长的角度出发，探讨如何利用大语言模型来辅导孩子的数学作业。我们会分享一些实用的策略和方法，帮助家长引导孩子与这些智能工具互动，提高他们的数学学习效率。

同时，我们也会讨论如何平衡技术辅导与亲子互动，确保孩子在获得学术帮助的同时，也能培养独立思考和自主学习的能力。通过本文，我们希望能够为家长们提供一份指南，让大语言模型成为您在孩子数学学习旅程中的得力助手。让我们一起探索如何将科技与家庭教育相结合，为孩子打造一个更加丰富、高效的学习环境。

以上简介是由大语言模型生成的，比我写的更好更宏大，接下来的内容是我以寻找解决一道数学题为缘由，问了几个 LLM 后的体验。如果有 AI 生成的内容我会标注。

### AI 新时代已经到来

本来这里也有一段 LLM 生成的介绍，免得嫌我啰嗦就删掉了。

如果你没有听说过最近的 AI 或者“大语言模型”，也没有关系，我相信的是只有普通百姓能用的 AI 才是真 AI。所以这篇文章没有任何技术性的东西，只有一些经验分享，而且是你可以直接上手用起来的那种。

下面正式进入我是如何使用 LLM 帮助分析一道数学题的内容。
### 这道数学题如下

请看图：

![](/posts/2024/solving-a-math-problem-by-llm/math-equation.jpg)

目前的大语言模型 AI 通常以聊天交互的方式来进行，大家尽管使用我们平常说话写文章的方式发过去就可以（专业术语叫做：自然语言）。

比如我是这样提问的：

```
请解决下面这道初中数学题，给出详细步骤并解释原因，以便学生能准确理解思路。公式和方程使用数学格式输出，结果是分数的保留分数：
若不论 k 取什么实数，关于 x 的方程 (2kx+a)/3-(x-bk)/6=1 的解总是 x=1，其中 a，b 是常数，则求 a+b 的值是多少？
```

如果问一些中文不太好的 AI，可以先让它翻译一下，方法是在上面文本的最后换行加上一句：

```
Please just translate the prompt to English.
```

然后再拿着英文的开一个新的对话。

```
Solve this math equation in a step by step way so that we can make sure the student can fully understand it. It's very important to me to solve it correctly. Please give more details as you can and reasons for each step too.
If no matter what real number k is, the solution to the equation (2kx+a)/3-(x-bk)/6=1 in terms of x is always x=1, where a and b are constants, then find the value of a+b.
```

再进一步，可以将数学格式用LaTEX语法装饰一下，当然，像上面的写法已经足够了，就看各家 AI 的理解能力了。

```
Please solve the following junior high school math problem, provide detailed steps, and explain the reasoning so that students can understand the solution accurately. Use LaTeX format for formulas and equations, and keep the result in the format of a fraction in simplest form:
For any real number k, the solution to the equation in $\frac{(2kx+a)}{3}-\frac{(x-bk)}{6}=1$, is always $x=1$, where $a$ and $b$ are constants. What is the value of $a+b$ ?
```

### 结果赏析

我使用了以下服务，点击链接可以直达结果分享或服务本身：

1. [通义千问](https://tongyi.aliyun.com/qianwen/share?shareId=c7d21ecf-616e-44fa-b5d1-b76bd07275c8)
2. [文心一言](https://yiyan.baidu.com/share/Ni5M3xcKCZ)
3. [Bing Copilot](https://sl.bing.net/dkbgk0Y7wfQ)
4. [Kimi Chat](https://kimi.moonshot.cn/share/co3q59pkqq4leau1fgug)
5. [零一万物开放平台](https://platform.lingyiwanwu.com/playground)
6. 本地模型 Gemma

来看看各自的表现，部分可以分享的能在文末找到链接。

#### 通义千问

结果正确。

额外功能：可以直接传给它图片，让它先读图，然后让他解题。家长们的福音有木有？

![](/posts/2024/solving-a-math-problem-by-llm/llm-qwen.png)

#### 文心一言

答案正确。

提供了有关联的继续追问问题，比如这里就提供了“如何理解 k 的系数必须为 0 的条件”，也是一个非常好的功能。

![](/posts/2024/solving-a-math-problem-by-llm/llm-yiyan.png)

#### Bing Copilot

答案正确。

![](/posts/2024/solving-a-math-problem-by-llm/llm-bingcopilot.png)

嗯，微软的字体渲染有些奇怪就是了 ：）

#### Kimi Chat

答案正确。

![](/posts/2024/solving-a-math-problem-by-llm/llm-kimichat.png)

同样提供了追问，有时间可以让孩子来一段打破砂锅问到底。

#### 零一万物开放平台

答案不正确。

![](/posts/2024/solving-a-math-problem-by-llm/llm-01platform.png)

据说新发布的 9B 模型增强了数学能力，等可用了再试试看吧。

#### 本地模型

我尝试了好几个模型，答案都不正确。看来还是要微调和“人在回路”。

![](/posts/2024/solving-a-math-problem-by-llm/llm-ollama.png)


### 感受总结

你觉得这些 AI 解题能力如何？

有几点我觉得很有意思：

- Prompt（提示语）其实就是自然语言，我们怎么讨论商量问题，就可以怎么给 AI 提问，重要的是思路，不是照搬提示工程模板之类。
- 礼貌用语的作用不是很大，但，粗鲁的语言却有反作用。一个小故事，前段时间在一个大模型的群里看到一个人给 AI 发的提示语简直不堪入目，很难相信他平常是那样跟人说话的，吓得我赶紧退群了。
- 通用模型的能力差别很大，使用的时候要注意自己有能力判断对错，当然，你也可以让模型自己检查自己，有时候也很好玩。

### 资源和链接

本地模型我使用了 Ollama。

也欢迎对 AI 和大模型感兴趣的一起聊聊。