# 你的 NLP 模型能够防止恶意攻击吗？

> 原文：<https://medium.com/hackernoon/does-your-nlp-model-able-to-prevent-adversarial-attack-45b5ab75129c>

## 对抗性攻击

![](img/badf9cfc3c9ace7bdffeb02f4a1f1728.png)

Photo by [Jeffrey F Lin](https://unsplash.com/@jeffreyflin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对抗性攻击是通过异常输入来欺骗模型的方法。Szegedy 等人(2013)在计算机视觉领域对其进行了介绍。给定一组正常的图片，优越的图像分类模型能够正确地对其进行分类。然而，同一模型不再对带有噪声(非随机噪声)的输入进行分类。