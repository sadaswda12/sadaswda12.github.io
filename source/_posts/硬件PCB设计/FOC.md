---
title: FOC控制
---

# 无刷电机控制原理

通过改变MOS管的开关来使三相轮替通电，从而达到旋转的目的

![electronic_style](/images/FOC/electronic_style.png)

![control](/images/FOC/control.png)

# 建立数学模型

## 克拉克变换

通过将三维模型简化位二维模型来简化计算，先将三相抽象为三个夹角为120°的三个向量，随后通过克拉克变换到二维坐标系阿尔法和贝塔轴上。

![clark_first](/images/FOC/clark_first.jpg)

当Ia电流为-1A时，Ic和Ib均为1/2A，经过变换得到I_阿尔法电流为-3/2，为了使电机模型简单，我们希望转换后 阿尔法电流 也是-1A，因此乘以2/3，此时就得到克拉克变换等幅值形式公式,对此公式利用基尔霍夫定律进行化简得到：

![clark_first](/images/FOC/clark_last.jpg)

## 克拉克逆变换

![clark_first](/images/FOC/contrary_clark.jpg)

## 帕克变换

阿尔法和贝塔坐标轴在定子上固定不动，但是背景的转子会转动造成电极变化使得电机旋转，帕克变换就是在定子上选取D-Q坐标系，Q指向N极。

同样利用sin和cos变换，将阿尔法-贝塔坐标系转换到D-Q坐标系下，就得到：

![clark_first](/images/FOC/Park_pos.png)

反变换就是：

![clark_first](/images/FOC/Park_neg.png)

# FOC代码编写

## 前置知识

![clark_first](/images/FOC/total_foc.png)