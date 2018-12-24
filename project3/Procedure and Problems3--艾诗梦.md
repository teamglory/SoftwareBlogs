实验要求：实现抠图，将superman.jpg 图像中的超人填到 space.jpg 中 

实验核心代码：	

	 for(i=0;i<imgYlen;i++)
	{	
		for(j=0;j<imgXlen;j++)
		{	
			if(!( buffTmpR1[i*imgXlen + j]<(GByte)160&& buffTmpR1[i*imgXlen + j]>(GByte)10 && buffTmpG1[i*imgXlen + j]<(GByte)220 && buffTmpG1[i*imgXlen + j]>(GByte)100 && buffTmpB1[i*imgXlen + j]<(GByte)110 &&buffTmpB1[i*imgXlen + j]>(GByte)10 ))
			{
				buffTmpR[i*imgXlen + j] = buffTmpR1[i*imgXlen + j];
				buffTmpG[i*imgXlen + j] = buffTmpG1[i*imgXlen + j];
				buffTmpB[i*imgXlen + j] = buffTmpB1[i*imgXlen + j];
			}
			else
			{
				buffTmpR[i*imgXlen + j] = buffTmpR2[i*imgXlen + j];
				buffTmpG[i*imgXlen + j] = buffTmpG2[i*imgXlen + j];
				buffTmpB[i*imgXlen + j] = buffTmpB2[i*imgXlen + j];
			}
		}
		
	}
实验中遇见的问题：在实验过程中，由于将superman图片填充到space图片的条件未考虑完全，导致图片显示不正确，即该条语句if(!( buffTmpR1[i*imgXlen + j]<(GByte)160&& buffTmpR1[i*imgXlen + j]>(GByte)10 && buffTmpG1[i*imgXlen + j]<(GByte)220 && buffTmpG1[i*imgXlen + j]>(GByte)100 && buffTmpB1[i*imgXlen + j]<(GByte)110 &&buffTmpB1[i*imgXlen + j]>(GByte)10 ))对三个通道没有同时判断，修改后图片显示正确

结果图片：![](https://github.com/Aishim/SoftwareClass/blob/master/super.png?raw=true)