简单抠图

实验要求：把superman.jpg中的超人抠出，加到 space.jpg 的太空背景中。

实验过程：
superman.jpg图像中的像素值用（r,g,b）表示，条件1：10<r<160; 条件2：100<g<220; 条件3：10<b<110；同时满足这三个条件的像素为超人区域像素。把超人区域像素，填到 space.jpg 中即可。

核心代码：

	for(j=0;j<supYlen;j++)
	{
		for(i=0;i<supXlen;i++)
		{
			if ( !(buffTmpSuperR[j*supXlen+i] > (GByte)10 && buffTmpSuperR[j*supXlen+i] < (GByte)160 && buffTmpSuperG[j*supXlen+i]>(GByte)100 && buffTmpSuperG[j*supXlen+i] < (GByte)220 && buffTmpSuperB[j*supXlen+i]>(GByte)10 && buffTmpSuperB[j*supXlen+i] < (GByte)110))
			{
				buffTmpSpaceR[j*supXlen + i] = buffTmpSuperR[j*supXlen + i];
				buffTmpSpaceG[j*supXlen + i] = buffTmpSuperG[j*supXlen + i];
				buffTmpSpaceB[j*supXlen + i] = buffTmpSuperB[j*supXlen + i];
			}

		}

	}

实验结果:
![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fxmcwv1o2ij30hr0ddn50.jpg)

遇到的问题：

实验中，将superman.jpg图像填到space.jpg 中时，没有同时满足三个条件，导致结果如图：
![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fxmdfy4dktj30ht0ddn5e.jpg)

将代码改成以下这样，就可以正确的实现。

if ( !(buffTmpSuperR[j*supXlen+i] > (GByte)10 && buffTmpSuperR[j*supXlen+i] < (GByte)160 && buffTmpSuperG[j*supXlen+i]>(GByte)100 && buffTmpSuperG[j*supXlen+i] < (GByte)220 && buffTmpSuperB[j*supXlen+i]>(GByte)10 && buffTmpSuperB[j*supXlen+i] < (GByte)110))

收获：
通过本次实验，学会了简单抠图，在审题过程中，更要细心，想的更全面一点。
