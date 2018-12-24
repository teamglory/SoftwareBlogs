----------------线性滤波-------------------------

对于下面的图像，使用GDAL编程，分别使用不同的核进行卷积，并观察效果
![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fy0taq0l05j30740740ty.jpg)

核心代码：

	for (i = 0 ; i < bandNum; i++)
    {
        poSrcDS->GetRasterBand(i+1)->RasterIO(GF_Read,
            0, 0, imgXlen, imgYlen, imgIn, imgXlen, imgYlen, GDT_Float32, 0, 0);
        if (flag == 1)
        {
            Box_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }
        else if (flag == 2)
        {
            Motion_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }
        else if (flag == 3)
        {
            Edge_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }
        else if (flag == 4)
        {
            Sharpness_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }
        else if (flag == 5)
        {
            Embossing_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }
        else if (flag == 6)
        {
            Gaussian_Filter(imgIn, imgOut, imgXlen, imgYlen);
        }

        poDstDS->GetRasterBand(i+1)->RasterIO(GF_Write,
            0, 0, imgXlen, imgYlen, imgOut, imgXlen, imgYlen, GDT_Float32, 0, 0);
    }
卷积核一：

![](https://wx2.sinaimg.cn/mw690/006K22Ivly1fy0tedynqsj306v05t3z2.jpg)


核心代码：

	for(j=1; j<imgYlen-1; j++)
	{
		for(i=1; i<imgXlen-1; i++)
		{
			imgOut[j*imgXlen+i] =(imgIn[(j-1)*imgXlen+i]          	                 
                                  +imgIn[j*imgXlen+i-1]
                                  +imgIn[j*imgXlen+i]
                                  +imgIn[j*imgXlen+i+1]                               
                                  +imgIn[(j+1)*imgXlen+i])/5.0f;
		}
	}

实验结果：均值模糊

![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fy0tbkjg1qj3074073n09.jpg)

卷积核二：

![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fy0tg44qg5j309v07amyz.jpg)



核心代码：

	 for (j=2; j<imgYlen-2; j++)
    {
        for (i=2; i<imgXlen-2; i++)
        {
			 imgOut[j*imgXlen+i] = (imgIn[(j-2)*imgXlen+i-2]
                                   +imgIn[(j-1)*imgXlen+i-1]
                                   +imgIn[j*imgXlen+i]                                   
                                   +imgIn[(j+1)*imgXlen+i+1]
								   +imgIn[(j+2)*imgXlen+i+2]) /5.0f;
        }
    }

实验结果：运动模糊

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fy0tbwmn4gj3074074tbr.jpg)

卷积核三：

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fy0tx7lr1zj304w04ldgd.jpg)

核心代码：

	for(j=1; j<imgYlen-1; j++)
	{
		for(i=1; i<imgXlen-1; i++)
		{
			imgOut[j*imgXlen+i] =(-imgIn[(j-1)*imgXlen+i-1]
            	                  -imgIn[(j-1)*imgXlen+i]
            	                  -imgIn[(j-1)*imgXlen+i+1]
                                  -imgIn[j*imgXlen+i-1]
                                  +imgIn[j*imgXlen+i]*8
                                  -imgIn[j*imgXlen+i+1]
                                  -imgIn[(j+1)*imgXlen+i-1]
                                  -imgIn[(j+1)*imgXlen+i]
                                  -imgIn[(j+1)*imgXlen+i+1]);
		}
	}
实验结果：边缘检测

![](https://wx2.sinaimg.cn/mw690/006K22Ivly1fy0tce8a4tj3076075tce.jpg)


卷积核四：

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fy0txfj6ihj305104v3z2.jpg)

核心代码：

	for(j=1; j<imgYlen-1; j++)
	{
		for(i=1; i<imgXlen-1; i++)
		{
			imgOut[j*imgXlen+i] =(-imgIn[(j-1)*imgXlen+i-1]
            	                  -imgIn[(j-1)*imgXlen+i]
            	                  -imgIn[(j-1)*imgXlen+i+1]
                                  -imgIn[j*imgXlen+i-1]
                                  +imgIn[j*imgXlen+i]*9
                                  -imgIn[j*imgXlen+i+1]
                                  -imgIn[(j+1)*imgXlen+i-1]
                                  -imgIn[(j+1)*imgXlen+i]
                                  -imgIn[(j+1)*imgXlen+i+1]);
		}
	}

实验结果：锐化

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fy0tcqxu5oj3074075gql.jpg)

卷积核五：

![](https://wx3.sinaimg.cn/mw690/006K22Ivly1fy0txmb3tej305504r74s.jpg)

核心代码：

	for(j=1; j<imgYlen-1; j++)
	{
		for(i=1; i<imgXlen-1; i++)
		{
			imgOut[j*imgXlen+i] =(-imgIn[(j-1)*imgXlen+i-1]
            	                  -imgIn[(j-1)*imgXlen+i]
                                  -imgIn[j*imgXlen+i-1]
                                  +imgIn[j*imgXlen+i+1]
                                  +imgIn[(j+1)*imgXlen+i]
                                  +imgIn[(j+1)*imgXlen+i+1]);
		}
	}

实验结果：浮雕

![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fy0td65o9jj3074075gp1.jpg)

卷积核六：

![](https://wx2.sinaimg.cn/mw690/006K22Ivly1fy0tycjbnvj30h005bwhs.jpg)

核心代码：

	for (j=2; j<imgYlen-2; j++)
    {
        for (i=2; i<imgXlen-2; i++)
        {
            imgOut[j*imgXlen+i] = (0.0120*imgIn[(j-2)*imgXlen+i-2] +
            	                   0.1253*imgIn[(j-2)*imgXlen+i-1] +
            	                   0.2736*imgIn[(j-2)*imgXlen+i] +
                                   0.1253*imgIn[(j-2)*imgXlen+i+1] +
                                   0.0120*imgIn[(j-2)*imgXlen+i+2] +
                                   0.1253*imgIn[(j-1)*imgXlen+i-2] +
                                   1.3054*imgIn[(j-1)*imgXlen+i-1] +
                                   2.8514*imgIn[(j-1)*imgXlen+i] +
                                   1.3054*imgIn[(j-1)*imgXlen+i+1] +
                                   0.1253*imgIn[(j-1)*imgXlen+i+2] +
                                   0.2763*imgIn[j*imgXlen+i-2] +
                                   2.8514*imgIn[j*imgXlen+i-1] +
                                   6.2279*imgIn[j*imgXlen+i] +
                                   2.8514*imgIn[j*imgXlen+i+1] +
                                   0.2763*imgIn[j*imgXlen+i+2] +
                                   0.1253*imgIn[(j+1)*imgXlen+i-2] +
                                   1.3054*imgIn[(j+1)*imgXlen+i-1] +
                                   2.8514*imgIn[(j+1)*imgXlen+i] +
                                   1.3054*imgIn[(j+1)*imgXlen+i+1] +
                                   0.1253*imgIn[(j+1)*imgXlen+i+2] +
                                   0.0120*imgIn[(j+2)*imgXlen+i-2] +
                                   0.1253*imgIn[(j+2)*imgXlen+i-1] +
                                   0.2736*imgIn[(j+2)*imgXlen+i] +
                                   0.1253*imgIn[(j+2)*imgXlen+i+1] +
                                   0.0120*imgIn[(j+2)*imgXlen+i+2]) / 25.0f;
        }
    }

实验结果：高斯模糊

![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fy0tdist7xj3072073776.jpg)

遇到的问题：

第一次不能正确输出图片，把GDT_Byte改成GDT_Float32，转换一下类型就好了

通过本次实验，了解了卷积的概念，学会了如何利用卷积运算将图形转化为自己想要的样子，虽然过程中遇到了问题，但也收获了许多。

