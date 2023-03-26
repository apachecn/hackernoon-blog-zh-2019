# 第 1 部分，共 2 部分:我如何在不到 2 小时的时间里制作出我的第一个 DIY 机械键盘

> 原文：<https://medium.com/hackernoon/part-1-of-2-how-i-built-my-first-diy-mechanical-keyboard-in-less-than-2-hours-b2c195885fe8>

## 组装硬件。也请阅读[第 2 部分](https://hackernoon.com/part-2-of-2-how-i-built-my-first-diy-mechanical-keyboard-in-less-than-2-hours-c1a50aa5d69)。

![](img/05bd2373c717e185cb3944ecf9541560.png)

这个系列教程的灵感很大程度上来自瑞恩和 [SydMeet 2018](https://www.youtube.com/watch?v=LKIoQqvakEU&t=10s) 。这些步骤是非常高级的，并且教程不包括关于机械键盘为什么和如何工作的科学部分。

# 材料:

![](img/450fff64560897000d521538258b6de0.png)![](img/1363f2210a3d4d4e162fbbb149f3074e.png)

Bought from Don (was also available on SydMeet2018 loot bags)

![](img/eb4f069665e54e5e39387569da8a214c.png)![](img/fde51db3ad606820e347bcda0f9cb1fe.png)![](img/5e4476ae6346570c5b0ae21089515c41.png)

Bought from Don (was also available on SydMeet2018 loot bags)

![](img/4f6a7d32889baef8806ed1afc0d3ead6.png)![](img/b1c086ab31b9b284f6a47673c51724f5.png)

Bought from KBDFans

![](img/97cc20a143ed6ba558e8921910a24533.png)![](img/a0ca4bdbe3a299ad9039ce520bc309e4.png)![](img/7ded3557824a7b2ab9e573566c25b00a.png)

# 第一步。将二极管焊接到 PCB

将二极管插入 PCB 之前，将其折叠。将所有 20 个二极管插入 Ds，二极管的深色部分应该进入正方形，二极管的浅色部分应该进入圆形。

![](img/e28b21e166665a799421082d37c40f44.png)![](img/72a7de671bea595693624ec8c6858104.png)

翻转到 PCB 的背面，焊接二极管的引脚，然后切掉多余的引脚。

![](img/4b1a57971f5254dfcaf7d13dea76a2d9.png)![](img/f646f57f920988096f5b398563927379.png)

# 第二步。插入稳定器

根据您要实现的键盘布局，此部分是可选的。在我的情况下，我想建立一个经典的数字垫，需要 3 个稳定器。

带有长金属的稳定器部分应该放在上面的大圆圈里，底部应该放在下面的小圆圈里。我觉得先填大圆圈比较容易。

![](img/6ee82f36b0f04bf88a09f847729d6e3e.png)![](img/6fd0f08c4f9869e48ab618c4713a411b.png)

Inserting stabilisers

![](img/473557d1bfc82fa45622410816eff9e1.png)![](img/6b2482fd280e463252b53135ca9a86e6.png)

Properly clipped stabilisers (front view— left, back view— right)

# 第三步。焊接引脚接头

将引脚接头插入 PCB。确保长引脚和二极管位于电路板的同一侧。

![](img/10d130246afc7959e2a0fa7bb351ec58.png)![](img/4882ea26adce8758a3663b979d274a64.png)

Inserting pin headers

确保引脚接头稳固，并且 pro-micro 可以在以后插入引脚接头。我试着随机应变，用临时准备的“左脚”踩板。单独焊接短引脚。

![](img/f3347bcf8ccaa16e3ff083d30f288bea.png)![](img/14164f96a37384a72f95084ba825f190.png)

# 第四步。焊接开关

将转角开关插入板中，通过在 PCB 上测试开关的插入，确保获得正确的方向。

![](img/10d57a119405bf5626d0f4cc9ba0a3b6.png)![](img/74bee7eae3d5f09bff728b4ee94be81e.png)

通过将角开关安装到 PCB 上，将板和 PCB 结合在一起。

![](img/3b6721ef02ed4c5439a3077632f6ad24.png)![](img/7596be3bd0570b673df2a601759f85fe.png)

首先焊接转角开关，为所有其他开关打下良好的基础。

![](img/deebabda8bbe0e6b9ad0fc4565954d31.png)![](img/4337219e5c0c07bc2358d206db51fbb1.png)

# 第五步。焊料亲微

将 pro-micro 添加到 PCB，方法是(1)将 PCB 的 TX 部分插入 pro-micro 的 TX 侧，以及(2)将 PCB 的 Raw 部分插入 pro-micro 的 Raw 侧。小心焊接每个引脚，因为这个部分*非常具有挑战性。*

![](img/79d00bfba5bc653f3c6b80912d221a20.png)![](img/a497755b5d20a89bd06d919bfd5168b5.png)

# 第六步。测试 pro-micro 是否通电

做一个快速测试，看看你是否在你的个人电脑上正确的焊接了 pro-micro。

![](img/3201779dca72ce1cae40d1c08ed7cc0c.png)

\o/ praise the soldering sprites \o/

有趣的事实:我忘了焊接一个开关。试着在上图中寻找。答案在[第二部分:软件配置](https://hackernoon.com/part-2-of-2-how-i-built-my-first-diy-mechanical-keyboard-in-less-than-2-hours-c1a50aa5d69)。哈哈哈！

[](https://hackernoon.com/part-2-of-2-how-i-built-my-first-diy-mechanical-keyboard-in-less-than-2-hours-c1a50aa5d69) [## 第 2 部分，共 2 部分:我如何在不到 2 小时的时间里制作出我的第一个 DIY 机械键盘

### 软件配置

hackernoon.com](https://hackernoon.com/part-2-of-2-how-i-built-my-first-diy-mechanical-keyboard-in-less-than-2-hours-c1a50aa5d69) 

# 来源:

*   [https://theboard.libsyn.com/](https://theboard.libsyn.com/?fbclid=IwAR26VRwr-Z_HypMp1qZJYdnUk-84K6VvVFiyhuKEChwCst2AYZvpW5rElic)
*   https://www.youtube.com/watch?v=_voKcLUjwv8
*   【https://config.qmk.fm/#/snagpad/LAYOUT_numpad_5x4 
*   [https://learn.sparkfun.com/tutorials/pro-micro-fio-v3-连接-指南/硬件-概述-pro-micro](https://learn.sparkfun.com/tutorials/pro-micro--fio-v3-hookup-guide/hardware-overview-pro-micro)
*   [https://www.keyboardtester.com/](https://www.keyboardtester.com/)