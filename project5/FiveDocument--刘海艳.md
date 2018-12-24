--------------------------------IHS遥感图像融合--------------------------------

将以下两个图基于ISH变换进行遥感图像融合

多光谱图像

![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fyhvv45yqyj30dp0dr17l.jpg)

全色图像

![](https://wx2.sinaimg.cn/mw690/006K22Ivly1fyhvvsig59j30dq0dqn4t.jpg)

核心代码：

 	for (i = 0; i < imgXlen*imgYlen; i++)
    {
        bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
        bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

        bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
        bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
        bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
    }

实验结果：

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fyhvwpmfx4j30dq0dqdun.jpg)

做实验开始不知如何在RGB和ISH之间进行转换，经过网络查询，RGB->ISH 间转换：
![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fyhwu298y1j305l027glh.jpg)

按照上面的方法得到I分量I _ new及中间量v1,v2（即H、S的分量）。

ISH->RGB 间转换：

用全色影像替代Ｉ分量，即bandP=I _ new,然后进行以下操作即可：

![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fyhwuiyrtzj306e0260sm.jpg)

通过本次实验，我掌握了如何将两个图像进行ISH遥感图像融合，同时也了解了除了RGB之外的另一种图像表示模型IHS，并学会了两种模型的转换。


