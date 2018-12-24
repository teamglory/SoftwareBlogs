project3       简单抠图

实验要求：把superman.jpg中的超人抠出，加到 space.jpg 的太空背景中。

实验过程： superman.jpg图像中的像素值用（r,g,b）表示，条件1：10<r<160; 条件2：100<g<220; 条件3：10<b<110；同时满足这三个条件的像素为超人区域像素。把超人区域像素，填到 space.jpg 中即可。

space.jpg

![Image text](https://github.com/lovelyfanzi/information/blob/master/space.jpg?raw=true)

superman.jpg

![Image text](https://github.com/lovelyfanzi/information/blob/master/superman.jpg?raw=true)

一开始感觉这个实验可能会很难，因为之前没有接触过相关知识。但是上网搜了搜发现还是可以完成的。

之前的实验里已经学会了原图像的读取、目的图像的建立，这次实验的关键就是对RGB三个通道的像素值进行判断:

```c++
	for(i=0; i<imgYlen; i++)
		for (j = 0; j < imgXlen; j++)
		{
			if (!(bufftmpR1[i*imgXlen + j] <(GByte)160 && bufftmpR1[i*imgXlen + j] > (GByte)10 && bufftmpG1[i*imgXlen + j] < (GByte)220 && bufftmpG1[i*imgXlen + j] > (GByte)100  && bufftmpB1[i*imgXlen + j] <(GByte)110 && bufftmpB1[i*imgXlen + j] > (GByte)10))
			{
				bufftmpR[i*imgXlen + j] = bufftmpR1[i*imgXlen + j];
				bufftmpG[i*imgXlen + j] = bufftmpG1[i*imgXlen + j];
				bufftmpB[i*imgXlen + j] = bufftmpB1[i*imgXlen + j];
			}
			else
			{

				bufftmpR[i*imgXlen + j] = bufftmpR2[i*imgXlen + j];
				bufftmpG[i*imgXlen + j] = bufftmpG2[i*imgXlen + j];
				bufftmpB[i*imgXlen + j] = bufftmpB2[i*imgXlen + j];
			}
		}
	
```

实验结果

output.tif

![Image text](https://github.com/lovelyfanzi/information/blob/master/output.tif)



在实验过程中遇到的问题：

一开始判断RGB通道的像素值时判断的不对，导致出不来正确的图像。