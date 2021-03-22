---
title: 英文写作笔记
date: 2021-03-18 20:46:26
updated: 2021-03-21 22:00:00
tags: Notes
categories: Notes
---

{% cq %}
《Bugs in Writing》读书笔记
{% endcq %}

<!--more-->

大老板推荐的书，挖坑。
本文主要记录自己的总结和理解，和书里的描述可能有出入。

### 被动语态

#### 避免被动语态

从读者的角度来看，一个句子的开头就是这个句子要表达的主体。
通常情况，我们会希望强调某个对象干了什么，例如：

> **（主动）** This project implements some features.
> **（被动）** Some features are implemented by this project.

读者看到被动语态的句子会很迷惑，这个句子好像要讲 some features，而不是 this project。
只有在少数情况，我们可以用被动语态来强调事件，如果具体的执行者不重要，例如：

> **（主动）** Someone's bug caused this incident.
> **（被动）** This incident was caused by someone's bug.

读者并不想知道这个 bug 干了什么，而是想知道这个 incident 是怎么回事。
其实语法都是对的，只是要明确你想表达和强调的主体是什么，同时优先使用主动语态。

#### 避免和预设动作混用

这里的混用是指，前半句使用 doing / to do 的形式，后半句使用被动语态。
通常情况，这种形式的前半句，是围绕后半句的主语进行描述的，例如：

> **（主动）** By using this framework, we can reduce our work.
> **（主动）** To reduce our work, we can use this framework.

当读者读到这样的句式，心里会预设相应的主题：

> 通过做一件事情，我们可以达到什么目的。
> 为了达到什么目的，我们应该做什么。

正常思路下，读者会认为，是『我们』做什么事，是『我们』为了达到什么目的。
如果这个时候换成了被动语态，就会多一个不必要的转折，造成混乱，例如：

> **（被动）** By using this framework, our work can be reduced.
> **（被动）** To reduce our work, this framework can be used.

读者会首先认为逗号前面的部分，是在说逗号后面的主语；但接着碰到被动语态又反应过来，前面不是在说这个主语。
那前面这个从句是在说什么呢？缺失了。