> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [medium.com](https://medium.com/the-node-js-collection/easily-identify-problems-in-node-js-applications-with-diagnostic-report-dc82370d8029)

> This blog post was originally published on IBM Developer and was contributed to the Node.js Collectio......

使用诊断报告轻松识别 Node.js 应用程序中的问题
===========================

[![](https://miro.medium.com/fit/c/96/96/1*K4iEdS-TbKfM1ub3aiN0ig.png)](https://nodejs.medium.com/?source=post_page-----dc82370d8029--------------------------------)[节点. js](https://nodejs.medium.com/?source=post_page-----dc82370d8029--------------------------------)跟随[](/m/signin?actionUrl=%2F_%2Fapi%2Fsubscriptions%2Fnewsletters%2F4de5c49bbc35&operation=register&redirect=https%3A%2F%2Fmedium.com%2Fthe-node-js-collection%2Feasily-identify-problems-in-node-js-applications-with-diagnostic-report-dc82370d8029&newsletterV3=96cd9a1fb56&newsletterV3Id=4de5c49bbc35&user=Node.js&userId=96cd9a1fb56&source=post_page-----dc82370d8029---------------------subscribe_user-----------)[2019 年 3 月 29 日](/the-node-js-collection/easily-identify-problems-in-node-js-applications-with-diagnostic-report-dc82370d8029?source=post_page-----dc82370d8029--------------------------------) · 7 分钟阅读[](/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc82370d8029&operation=register&redirect=https%3A%2F%2Fmedium.com%2Fthe-node-js-collection%2Feasily-identify-problems-in-node-js-applications-with-diagnostic-report-dc82370d8029&source=post_actions_header--------------------------bookmark_preview-----------)

_这篇博文最初发表在_ [_IBM Developer 上_](https://developer.ibm.com/articles/easily-identify-problems-in-your-nodejs-apps-with-diagnostic-report/)_，并由 Gireesh Punathil 贡献给 Node.js Collection。_

诊断报告在 Node.js 核心中可用

该[诊断报告工具](https://nodejs.org/dist/latest-v11.x/docs/api/report.html)最近带入 Node.js 的核心，帮助开发人员识别生产几乎所有情况下的 Node.js 应用程序异常。场景包括异常终止，如崩溃、性能缓慢、内存泄漏、高 CPU、意外错误、错误输出等。

虽然该报告没有指出确切的问题或具体的修复，但其内容丰富的诊断数据提供了有关问题的重要提示并加速了诊断过程。

该实用程序最初作为 [npm 模块提供](https://www.npmjs.com/package/node-report)，并被引入 Node.js 核心，因为它极大地帮助确定了多种类型问题的根本原因，包括发送到 Node.js 组织中不同存储库的支持问题。在它成为核心的一部分之前，您必须明确地将依赖项添加到用户应用程序中的 npm 模块，这是采用诊断工具的障碍。

在这篇博文中，我描述了为什么工具很重要，然后详细介绍了这个如何解释报告，并在文章的最后，向您介绍一些示例数据。

常见的诊断步骤
=======

通常，诊断应用程序问题的起点是：

*   以了解开发的执行环境
*   可根据您获得的信息定义调试策略
*   执行一项或多项调查步骤。根据前面步骤的推断，每一步都可能改变您寻找或执行的操作
*   直到当前理论被具体的数据指标

Node.js 部署的问题确定涉及许多不同的工具和方法。

例如，如果您的应用程序崩溃，您将：

*   将故障转储加载到调试器中
*   检查失败线程中的失败背景
*   向后遍历执行序列或数据流向异常，以及负责的上述代码或数据流的应用程序、代码流、运行时代码或配置。

与内存供应相关的问题，这些可能会有所不同。

生产级部署中的问题确定问题
=============

对于生产级部署，我上面概述的诊断问题的方法带来了很多挑战，特别是：

*   **意外的业务影响。**这些步骤可以是迭代的，如果遵循，可能会对生产系统所代表的业务造成不可接受的影响。即使它们不是迭代的，它们也会导致调试任务延迟。例如，下一次数据捕获需要下一次回收，这需要数周时间。
*   这些步骤很**容易出现人为错误。**这些步骤很**容易出现错误。**这些步骤很容易出错，具体的水平诊断他们的技术人员的技能。例如，在站点生产这些步骤的 IT 专家对执行 Node.js 简直是诊断成功。
*   **知识差距。**很多时候，需要在系统上设置额外的设置来收集数据。例如，您可能必须更改的ulimit设置以启用转储或启用GC日志记录以收集堆统计信息和GC活动。
*   **数据收集限制。**很多时候，需要的数据可能无法无法收集。系统收集数据的示例包括：事件循环有多少柄？那些是在什么州？节点可能文件链接到哪个 SSL 库？
*   **二进制数据障碍。**由于数据是以二进制格式收集的，很多时候，用户或管理员不知道正在执行什么调试过程或正在收集什么数据。有时很难获得批准将二进制数据转发给可以解决问题的开发人员，而不是可以更容易地审查和隐私的格式。

解决方案
====

该解决方案是特定的文档，它有与您特定的执行相关的最常见的诊断数据。诊断报告使用了首次故障数据提供 (FFDC) 执行此操作。本文档采用半人机主题的格式，因此，如果您对诊断报告有更深入的了解，您可以在其原始状态下阅读它，也可以将其加载到 JS 程序中或传递给监控代理。

本文档可以改善整体故障检修体验，因为它：

*   回答很多经常问题，可以减少了解失败原因需要的循环次数。
*   提供故障时应用程序和虚拟机状态的综合视图。如果需要，此信息可以深入地考虑下一组数据收集的决策制定。

理想情况下，FFDC 可以让某人在没有任何额外信息的情况下解决问题！

功能
==

诊断报告是一个内置于Node.js的核心的实验性工具。它的功能是生成关于应用程序不当行为点或用户有兴趣获取更多信息的点的JSON文档。生成的文档包含有关应用程序和托管平台状态的信息，查看所有重要的数据元素。

下面命令行参数运行诊断报告（还有很多其他的，但这是一个）。

```
$ node--experimental-report --diagnostic-report-uncaught-exception w.js
将 Node.js 报告写入文件：report.20190309.102401.47640.001.json 
Node.js 报告完成

```

它捕获的数据可能与异常有关，例如终止程序的致命错误、应用程序异常或任何其他常见故障情况。这些工具实际捕获的数据可能是 JavaScript 堆统计信息、本机和应用程序调用堆栈、进程的 CPU 消耗等。

一些命令行参数可用于控制报告生成触发器和报告生成行为。

$ node –helpgrep report`--experimental-report`enable report generation`--diagnostic-report-on-fatalerror`generate diagnostic report on fatal (internal) errors`--diagnostic-report-on-signal`generate diagnostic report upon receiving signals`--diagnostic-report-signal=...`causes diagnostic report to be produced on provided signal. Unsupported in Windows. (default: SIGUSR2)`--diagnostic-report-uncaught-exception`generate diagnostic report on uncaught exceptions`--diagnostic-report-directory=...`define custom report pathname. (default: current working directory of Node.js process)`--diagnostic-report-filename=...`define custom report file name. (default: YYYYMMDD.HHMMSS.PID.SEQUENCE#.txt)

You can also generate the report explicitly via an API which is exposed through the Node.js process object. When using the API, the report is available both as a disk file or a JSON string. Another API controls the report generation triggers and reports generation behaviors.

Use cases
=========

In this section, we illustrate some of the benefits of Diagnostic Report through a few different use cases. Keep in mind that this list isn’t exhaustive. Diagnostic Report is a general-purpose tool that can be used in any problem scenarios.

1.  Identify which SSL library the current Node installation is linked against (roughly identify the distribution).

In this case, you can produce a report through the process.report.writeReport() API. As the following image shows, the component versions section contains the `SSL` linkage information. In this case, it is linked against version 1.1.1b of openssl (line 12).

![](https://miro.medium.com/max/1100/0*M6Hs089ICIGTOeLZ.png)

Reviewing the shared libraries that Node is linked against, you see no external SSL libraries in the list. From there, you can conclude that this is a standard community distribution.

![](https://miro.medium.com/max/1300/0*q5oeDv4m8Uw9WIW8.png)

2. A Node application hangs when you expect it to complete some tasks and then terminate. You have no idea what is causing the event loop to engage.

In this case, produce the report by sending a SIGUSR2 signal to the running process.

![](https://miro.medium.com/max/1400/0*kyHnaDFGZYygSaW4.png)

The generated report shows an active timer handle lying in the loop that has an expiry time of around 10 hours from the current time. (You can see that on line 4; “firesinMSfromNow” shows how many milliseconds it takes to fire).

Because the application should not be scheduling an event for 10 hours in the future, you now understand the reason for hang. To fix, search in the application that installs a setTimeout handler with the said duration.

3. You to make sure a web application that you host in a cloud environment is idle outside of business hours.

In this case, you again produce a report by logging into the cloud instance through SSH and sending a signal to the running process. The report generated in the persistent volume showed the resource usage section:

![](https://miro.medium.com/max/1400/0*5qcs3Va2_Pxe-LUy.png)

Lines two and three show the time spent by the node process in the user space and kernel space. Because they were only a fraction of a second spent, you can be confident that the application is relatively idle, with no file system activities in the recent past.

We need your feedback
=====================

Diagnostic Report is available as an experimental feature from Node.js v11.8.0 and subsequent releases. The tool could exit the experimental status and become a stable and supported feature, based on:

*   The perceived usability in the field
*   Any fine-grained tuning that may be required at the API interface level.

Again, this is based on user feedback.

在软件开发中，“该功能冻结”是无法细化的界面，因为它们已经在大量使用，并且在它们已经构建了许多软件；对接口的任何改变都可能破坏所有这些。

为了避免使用我们的诊断报告工具诊断功能，我们要求您在使用机会时尽快评估此功能，并直接在[Node.js 诊断用户反馈库中](https://github.com/nodejs/diagnostics/issues/280)提供研究人员的反馈。

_本教程记录了 v11.12.0 中的 Node.js API。_