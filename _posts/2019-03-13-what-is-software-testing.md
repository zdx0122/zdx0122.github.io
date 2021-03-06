---
layout: post
title:  "什么是软件测试？"
date:   2019-03-13 
categories: 软件测试教程
tags:  软件测试教程 软件测试
author: i.itest.ren
---

* content
{:toc}

在这篇文章中，我们将进行软件测试的概述。
我们将回答一些与软件测试相关的常见问题，并将通过4W1H的方式讨论测试。







在这篇文章中，我们将进行软件测试的概述。
我们将回答一些与软件测试相关的常见问题，并将通过4W1H的方式讨论测试。

注：4W1H：What, Why, Who, When and How

![什么是软件测试](http://wx3.sinaimg.cn/large/6b6f6a6fgy1g11l22fltkj20vq0hstf5.jpg)

## 什么是软件测试？

软件测试是评估系统的过程，其目的是发现错误。执行此操作以检查系统是否满足其指定要求。
测试在其正确性，完整性，可用性，性能以及其他功能和非功能属性方面测量系统的整体质量。

《软件测试的艺术》中定义为，**测试是为发现错误而执行程序的过程。**

## 为什么需要测试？

测试是软件工程生命周期中重要的一环，因为：

- 测试为利益相关者提供产品按预期交付的保证。
- 在没有经过适当测试的情况下泄露给最终用户的可避免缺陷会给企业带来不良信誉。
- 单独的测试阶段为利益相关者增加了关于所开发软件质量的保障。
- 在软件工程生命周期中，越早发现的问题修复后，花费的成本和资源会越低。
- 通过检测早期开发阶段的问题来节省开发时间。
- 测试团队通过为产品开发过程提供不同的观点，为软件开发增加了另一个维度。

总之，测试可以减少错误，以免付出昂贵的代价。

## 谁做测试？

软件测试是可以由与软件项目相关的所有技术和非技术人员完成。

各阶段的测试均由以下各项完成：
- 开发人员：开发人员对软件进行单元测试、冒烟测试，并确保各个方法正常工作。
- 测试人员：专门做软件测试的人员。功能测试人员做业务测试、手工测试，验证和保障程序的功能正确性；性能测试人员对系统进行并发压力性能测试，验证和保障程序的性能；自动化测试人员是用脚本或者代码对测试用例进行自动化，提升手工测试效率；测试开发人员是开发测试工具、平台，为其他测试人员提升效率。
- 测试经理/主管：定义测试策略和测试计划，定义团队内的测试规范、标准等。
- 最终客户/产品经理：最终客户或者产品经理执行程序的用户验收测试（UAT），以确保软件可以在现实世界/生产环境中正常工作。


## 什么时候开始软件测试？

基于软件项目的不同软件开发生命周期模型的选择，测试阶段在不同阶段开始。常见模型的有瀑布流模型、V模型等。

一般来说，会在开发人员把项目的代码进行提测后，开始介入功能测试，但在提测前会完成测试计划、方案，也会把测试用例进行多方评审完成。用例评审通常是项目中相关的产品人员、开发人员、测试人员一起参加评审。

## 如何完成软件测试？

软件测试既可以手工测试完成，也可以使用自动化工具完成。手工工作包括验证要求和设计；制定测试策略和计划; 准备测试用例然后执行测试。自动化工作包括为UI自动化，后端自动化，性能测试脚本准备和其他自动化工具的使用准备测试脚本。