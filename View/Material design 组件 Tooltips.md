# Material design 组件 Tooltips
---
> **Tooltips**: 工具提示,当用户鼠标 悬停`接触`定焦于某个元素时显示的**文本标签**.
> 此为译文,原文:[Material Design Tooltips 官网地址](https://material.io/guidelines/components/tooltips.html#)

### 内容

当Tooltips被触发时,它会鉴定该元素,并提供描该元素功能的简短文字描述.例如:它可以展示可点击图标的功能用途的文字信息.

**Tooltips 不能获取焦点.**

以下情况Tooltips 显示:
- 当鼠标悬停在一个元素上
- 用键盘定焦在某个元素上(通常是`Tab`键)
- 触摸


![Tooltips](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips%20-%20Components%20-%20Material%20design%20guidelines%283%29.png)

### 使用

使用Tooltips的交互式图像

![示例1](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips-1.png)

工具提示不显示丰富的信息，包括图像和格式化文本
Tooltips 与 [ALT属性元素](https://zh.wikipedia.org/zh/Alt%E5%B1%AC%E6%80%A7)不同,其(ALT属性元素)主要显示**静态图像**.
Tooltips 没有方向箭头,取而代之是用元素所在的位置产生动作去展示.

![示例2](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips-2.png)

### 交互

Tooltips 是通过点击并按住元素触发. 只要用户继续保持操作,Tooltips就会持续展示

**时间**

展示时间 **1.5s**
如果用户在当前元素展示结束之前触发了另一个元素,那么Tooltips会(提前)消失

**运动细节**

Tooltips 显示时使用 **150ms 减速曲线运动**，它消失时使用**150ms 加速曲线运动**

### Tooltips(桌面端)

文字: 10sp (中等大小)
字体: Roboto
颜色: Grey700
不透明度: 90%

![桌面端-1](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips%20-%20Components%20-%20Material%20design%20guidelines%287%29.png)
鼠标/键盘的 Tooltips

高度: 22dp
左右内间距: 8dp
上外边距: 14dp

![桌面端-2](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips%20-%20Components%20-%20Material%20design%20guidelines.png)
鼠标/键盘 Tooltips 示例

### Tooltips(手机端)

文字: 14sp (中等大小)
字体: Roboto
颜色: Grey700
不透明度: 90%


![手机端](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips%20-%20Components%20-%20Material%20design%20guidelines%286%29.png)
触摸时Tooltips的UI展示

高度: 32dp
左右内间距: 16dp
上外边距: 24dp

![最后的示例](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/22/Tooltips-3.png)

> 后记:这就是英语不过6的下场.o(*≧▽≦)ツ