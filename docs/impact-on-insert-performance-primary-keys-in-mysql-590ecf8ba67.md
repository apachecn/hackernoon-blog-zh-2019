# 对插入性能的影响 MySQL 中的主键

> 原文：<https://medium.com/hackernoon/impact-on-insert-performance-primary-keys-in-mysql-590ecf8ba67>

## MySQL 中主键的重要性

*这篇文章最后一次更新是在 2021 年 10 月 12 日。请注意，我已经交替使用了 MySQL 和它的存储引擎 InnoDB。*

![](img/a3eaf45c7997b2ac4f798bd3f0bc0fb2.png)

A 几天前，工作中有人问我，当桌子上有`primary key`在场时，在桌子上插入是否会更快。我的直接回答是…