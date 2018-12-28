project4

一、实验要求：

![](https://github.com/lovelyfanzi/information/blob/master/lina.jpg?raw=true)

对于上面的图像，使用GDAL编程，分别使用下面的核进行卷积，并观察效果，说明结果是哪一种滤波。

![](https://github.com/lovelyfanzi/information/blob/master/request.jpg?raw=true)

二、实验步骤

1.读取图像

2.对读取的图像进行卷积处理

先定义两种卷积运算的函数，一个是3x3矩阵的，一个是5x5矩阵的

```
double matirx(double a[5][5], double b[5][5])
{
	double result = 0;
	for (int i = 0; i < 5; i++)
	{
		for (int j = 0; j < 5; j++)
		{
				result += a[i][j] * b[i][j];
		}
	}
	return result;
}
double matirx(double a[3][3], double b[3][3])
{
	double result = 0;
	for (int i = 0; i < 3; i++)
	{
		for (int j = 0; j < 3; j++)
		{
				result += a[i][j] * b[i][j];
		}
	}
	return result;
}
```

再轮流进行卷积运算

```
			for (n = 0; n < imgXlen; n++)
			{
				if (m >= 1 && m <= imgYlen - 2 && n >= 1 && n <= imgXlen - 2)
				{
					for (int l1 = 0; l1 < 3; l1++)
					{
						for (int l2 = 0; l2<3; l2++)
							example1[l1][l2] = bufftmp[i][(m + (l1 - 1))*imgXlen + n + (l2 - 1)];
					}
					result[0] = (int)(0.2*matirx(example1, matrix1));
					result[2] = (int)(matirx(example1, matrix3));
					result[3] = (int)(matirx(example1, matrix4));
					result[4] = (int)(matirx(example1, matrix5));
				}
				else
				{
					result[0] = 0;
					result[2] = 0;
					result[3] = 0;
					result[4] = 0;
				}
				if ( m>=2 && m<=imgYlen-3 && n>=2 && n <= imgXlen - 3)
				{
					for (int l1 = 0; l1 < 5; l1++)
					{
						for(int l2=0;l2<5;l2++)
						example2[l1][l2]=bufftmp[i][(m +(l1-2))*imgXlen + n +(l2-2)];
					}
					result[1] = (int)(0.2*matirx(example2, matrix2));
					result[5] = (int)(matirx(example2, matrix6)/25);
				}
				else
				{
					result[1] = 0;
					result[5] = 0;
				}
				for (int l3 = 0; l3 < 6; l3++)
				{
					if (result[l3] < 0)    result[l3] = 0;
					else if (result[l3] > 255) result[l3] = 255;
					bufftmp1[l3][i][m*imgXlen + n] = (GByte)result[l3];
				}
			}
```

得到的六种结果分别是均值模糊、运动模糊、边缘检测、图像锐化、浮雕处理、高斯模糊。结果如下：

1.均值模糊

![](https://github.com/lovelyfanzi/information/blob/master/output1.jpg?raw=true)

2.运动模糊

![](https://github.com/lovelyfanzi/information/blob/master/output2.jpg?raw=true)

3.边缘检测

![](https://github.com/lovelyfanzi/information/blob/master/output3.jpg?raw=true)

4.图像锐化

![](https://github.com/lovelyfanzi/information/blob/master/output4.jpg?raw=true)

5.浮雕处理

![](https://github.com/lovelyfanzi/information/blob/master/output5.jpg?raw=true)



6.高斯模糊

![](https://github.com/lovelyfanzi/information/blob/master/output6.jpg?raw=true)