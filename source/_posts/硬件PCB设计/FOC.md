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