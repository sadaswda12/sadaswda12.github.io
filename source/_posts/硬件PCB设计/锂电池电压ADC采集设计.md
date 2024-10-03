---
title: 锂电池电压ADC采集设计
---

普通的分压设计除非使用拨动开关将这部分电路完全断电，否则会导致锂电池通过分压电阻在持续耗电，当耗电量达到一定程度会导致锂电池损坏。

本例子采用航模电池2s电池为7.4V，分压最大为3.22V，符合单片机的3.3V电压。

![image_ADC](/images/锂电池电压ADC采集设计_img/image_ADC.png)

在使用时候，在设备关机后，将引脚拉高，利用PMOS将电路断开，有效避免了分压电阻耗电。

```c
//ADC读取电压
u16 Get_Adc_Average(u8 times)
{
    u32 temp_val = 0;
    u8 t;

    for(t = 0; t < times; t++)
    {
		HAL_ADC_Start(&hadc);
		HAL_ADC_PollForConversion(&hadc, 1000);
        temp_val += HAL_ADC_GetValue(&hadc);
        delay_ms(5);
    }

    return temp_val / times;
}

```

经过这部分操作后得到的数据并不是电池电压值，这就需要了解 几位的ADC。在STM32中，ADC是12位的，另外，8、10、12位都有。

对于STM32，ADC的值是0~4095(十二位二进制数表示的最大数是4095).这样我们就可以利用一次函数的样子来求解任意X坐标下的Y值了。

```c
BatADCValue=Get_Adc_Average(10); // 获取ADC原始值0~4095
BatValue= (float)BatADCValue * (3.3 / 2315); //得到电压值    为什么是2315？因为电阻分压了,需要计算下面电阻占的比例*4096
printf("%.1fV\r\n",BatValue);
BatADCValue = BatValue*100; // 把float数据给了int
printf("%dV\r\n",BatADCValue);
```

最终就得到我们需要的电池电压了！



