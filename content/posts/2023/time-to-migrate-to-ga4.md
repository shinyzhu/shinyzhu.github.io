---
title: "是时候迁移 Google 分析到 GA4 了"
description: "It's time to migrate your Google Analytics to the new GA4."
featured_image: "/posts/2023/time-to-migrate-to-ga4/ga4-countdown.png"
date: 2023-06-20T13:40:00+08:00
lastmod: 2023-06-20
categories: DevTool
tags: [google, analytics, seo]
comment: true
---

# 是时候迁移 Google 分析到 GA4 了

网站统计系统千千万，最终还是要用回 Google Analytics。我记得很早之前使用阿江统计，那还是 ASP 的时代。后来自己建博客就一直使用 Google 分析了，虽然也断断续续使用过别的一些系统，但最终还是 GA 是这个领域的标杆。

如今，标杆要升级换代了。

其实已经有过几次升级了，比如最早用 `analytics.js` 的统计代码，然后升级到 `gtag.js` 代码。

如果你还在使用 Google 分析的 Universal Analytics (UA) 版本，进入分析报表将会收到倒计时提示了，即 UA 版本的数据收集将在7月1号正式停止，官方建议迁移到[新的 GA4 版本](https://support.google.com/analytics/answer/10089681)。

> 注意：本文不是迁移教程，仅仅记录我自己的一些操作过程。

先看看官方对 GA4 的定义和说明：

> GA4 是专为未来的衡量方式而设计的新型[媒体资源](https://support.google.com/analytics/answer/9355666)：
>
> - 收集网站数据和应用数据，有助于更好地了解客户转化历程
> - 使用基于事件（而非基于会话）的数据
> - 提供如“无 Cookie 衡量”等隐私控制项，以及行为建模和根据模型估算转化
> - 具备无需构建复杂模型即可预测的能力，可发挥指导作用
> - 与媒体平台直接集成，有助于吸引用户在您的网站或应用上采取行动
>
> **2023 年 7 月 1 日**起，标准 Universal Analytics 媒体资源将不再处理数据。在 2023 年 7 月 1 日之后的一段时间内，您仍然可以查看 Universal Analytics 报告。但是，新数据只会传入 Google Analytics（分析）4 媒体资源。[了解详情](https://support.google.com/analytics/answer/11583528)

## 创建新的 GA4 资源

根据官方的[后续安排](https://support.google.com/analytics/answer/11583528)，你现在应该已经有一个自动创建和迁移的 GA4 资源了，如果没有可以自己创建一个新的 Property。我的是之前自己创建的。创建好之后有如下步骤来配置基本信息。

## 配置数据流

在管理界面的 Property 下，选择 GA4 资源，可以看到列表有些不一样。找到 Data Streams 就是配置数据流，即分析系统从哪里获取数据，GA4 的优点之一是可以将 Web 和 App 的行为数据采集到同一个资源下。

![ga4-data-streams](/posts/2023/time-to-migrate-to-ga4/ga4-data-streams.png)

配置好后会拿到一个 Measurement ID，这个就是用来更新我们 Web 或 App 的跟踪代码的。

## 更新 Tracking 代码

还是在上一个界面，在最下面可以看到 View tag instructions，点开就是如何安装跟踪代码的提示。

提供了一些 CMS 系统的自动安装，以及手动更新的代码块。看起来像这个样子：

```js
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-6BNX000R1H"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-6BNX000R1H');
</script>
```

你可以把这段代码替换之前的跟踪代码。

> 注意：请复制你自己的代码或替换成自己的跟踪 ID。

### Hugo 如何更新 GA 跟踪代码？

我的博客是用的 Hugo 搭建，配置 GA 是设置跟踪代码即可：

```toml
//file: config.toml
googleAnalytics = "G-6BNX000R1H"
```

然后发布就可以正常工作了。

[如果不行](https://github.com/AmazingRise/hugo-theme-diary/issues/154)，可以等待一段时间看看有没有数据。

所有以上步骤都在“[设置助手](https://support.google.com/analytics/answer/10110290)”里有详细的提示。

## 关联 BigQuery

这一步是可选的，不做也完全没关系。

但如果你跟我一样，想对采集的事件数据进行深入的探索，官方提供了关联到一个 BigQuery 项目。

> BigQuery 是一个云数据仓库，可供您迅速查询大型数据集。
>
> 您可以将 Google Analytics（分析）4 媒体资源中的所有原始事件导出到 BigQuery，然后使用类似 SQL 的语法查询这些数据。在 BigQuery 中，您可以将数据导出至外部存储空间，或者导入外部数据以与您的 Google Analytics（分析）数据合并。

在设置助手的下方，展开那个 Advanced setup (optional)，找到 Link to BigQuery，就可以关联已有的 API 项目了。

等等。

如果你没有 API 项目，就需要先新建一个：

1. 登录 [Google 云的控制台](https://console.cloud.google.com/)。
2. 创建新项目。
3. 在 API 和服务里搜索到 [BigQuery API（你当然可以直接点击链接）](https://console.cloud.google.com/apis/library/bigquery.googleapis.com)。
4. 启用 BigQuery。

回到刚才的设置，可能需要刷新一下页面。然后选择关联的 BigQuery 项目就可以了。这里我发现有两种数据同步频率：**按天**或**流式**。流式需要设置账单，我就使用按天好了。

## 我的报表呢？

![](/posts/2023/time-to-migrate-to-ga4/ga-ga4.png)

如果一切正常，你应该就可以看到新的报表了。

我发现报表才是这次升级里最大的变化，等我继续熟悉新的报表吧。

`// TODO: Write a blog to introduce the new reporting in GA4.`
