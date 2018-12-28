----------------------------遥感图像的分块处理-------------------------------------

使用上周学习的IHS方法，对下面两种图像分别使用两种分块方式进行融合：

Mul_large.tif：

![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fyl9mcka73j30hw0doqlz.jpg)

Pan_large.tif：

![](https://wx2.sinaimg.cn/mw690/006K22Ivly1fyl9l8t81bj30hz0dqgus.jpg)

第一种方式：使用256 * 256 大小的块；

第二种方式：每块的宽度为图像宽度，高度为256像素。

比较两种分块方式的处理效率。

第一种方式：

核心代码：

    int m=imgYlen/256+1;
	int n=imgXlen/256+1;
	int n1=1,m1=1;
	while(m1<m)
	{
		n1=1;
		while(n1<n)
		{
			//读取图像缓存
			poSrcDS1->GetRasterBand(1)->RasterIO(GF_Read, 256*(n1-1), 256*(m1-1), 256, 256,
				bandR, 256, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(2)->RasterIO(GF_Read, 256*(n1-1), 256*(m1-1), 256, 256,
				bandG, 256, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(3)->RasterIO(GF_Read, 256*(n1-1), 256*(m1-1), 256, 256,
				bandB, 256, 256, GDT_Float32, 0, 0);
			poSrcDS2->GetRasterBand(1)->RasterIO(GF_Read, 256*(n1-1), 256*(m1-1), 256, 256,
				bandP, 256, 256, GDT_Float32, 0, 0);
			//RGB与IHS间的变换
			for (i = 0; i < 256*256; i++)
			{
				bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
				bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

				bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
				bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
				bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
			}

			poDstDS->GetRasterBand(1)->RasterIO(GF_Write, 256*(n1-1), 256*(m1-1), 256, 256,
				bandR, 256, 256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(2)->RasterIO(GF_Write, 256*(n1-1), 256*(m1-1), 256, 256,
				bandG, 256, 256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(3)->RasterIO(GF_Write, 256*(n1-1), 256*(m1-1), 256, 256,
				bandB, 256, 256, GDT_Float32, 0, 0);
			n1++;
		}
		m1++;
	}
	n1=1;
	while(n1<n)
	{
	        //读取图像缓存
			poSrcDS1->GetRasterBand(1)->RasterIO(GF_Read,  256*(n1-1),  256*(m-1), 256, imgYlen%256,
				bandR,  256, imgYlen%256,GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(2)->RasterIO(GF_Read,   256*(n1-1), 256*(m-1), 256, imgYlen%256,
				bandG, 256, imgYlen%256,GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(3)->RasterIO(GF_Read,   256*(n1-1), 256*(m-1), 256, imgYlen%256,
				bandB, 256, imgYlen%256, GDT_Float32, 0, 0);
			poSrcDS2->GetRasterBand(1)->RasterIO(GF_Read,   256*(n1-1), 256*(m-1), 256, imgYlen%256,
				bandP,  256, imgYlen%256,GDT_Float32, 0, 0);
			//RGB与IHS间的变换
			for (i = 0; i < 256* (imgYlen%256); i++)
			{
				bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
				bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

				bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
				bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
				bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
			}

			poDstDS->GetRasterBand(1)->RasterIO(GF_Write,256*(n1-1),  256*(m-1), 256, imgYlen%256,
				bandR, 256, imgYlen%256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(2)->RasterIO(GF_Write, 256*(n1-1), 256*(m-1), 256, imgYlen%256,
				bandG, 256, imgYlen%256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(3)->RasterIO(GF_Write, 256*(n1-1), 256*(m-1), 256, imgYlen%256,
				bandB, 256, imgYlen%256, GDT_Float32, 0, 0);
			n1++;
	}


第二种方式：

核心代码：

    int m=imgYlen/256+1;
	int n=1;
	while(n<m)
	{
			//读取图像缓存
			poSrcDS1->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(n-1), imgXlen, 256,
				bandR,  imgXlen, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(2)->RasterIO(GF_Read,   0, 256*(n-1), imgXlen, 256,
				bandG,  imgXlen, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(3)->RasterIO(GF_Read,  0, 256*(n-1), imgXlen, 256,
				bandB,  imgXlen, 256, GDT_Float32, 0, 0);
			poSrcDS2->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(n-1), imgXlen, 256, 
				bandP,  imgXlen, 256, GDT_Float32, 0, 0);
			//RGB与IHS间的变换
			for (i = 0; i < imgXlen * 256; i++)
			{
				bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
				bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

				bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
				bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
				bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
			}

			poDstDS->GetRasterBand(1)->RasterIO(GF_Write, 0, 256*(n-1), imgXlen, 256,
				bandR, imgXlen, 256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(2)->RasterIO(GF_Write, 0, 256*(n-1), imgXlen, 256, 
				bandG, imgXlen, 256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(3)->RasterIO(GF_Write, 0, 256*(n-1), imgXlen, 256,
				bandB, imgXlen, 256, GDT_Float32, 0, 0);
			n++;
	}
	//读取图像缓存
			poSrcDS1->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandR,  imgXlen, imgYlen-(m-1)*256,GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(2)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandG,  imgXlen, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(3)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandB,  imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poSrcDS2->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandP,  imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			//RGB与IHS间的变换
			for (i = 0; i < imgXlen * (imgYlen-(m-1)*256); i++)
			{
				bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
				bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

				bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
				bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
				bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
			}

			poDstDS->GetRasterBand(1)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandR, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(2)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandG, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(3)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandB, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);

实验结果：

![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fyl9dbje3lj30hz0dqng2.jpg)

在实验的过程中，遇到很多问题，例如，开始时输出的结果图是下面这样的：

![](https://wx4.sinaimg.cn/mw690/006K22Ivly1fykkoyw409j30hz0doaag.jpg)

它只读了第一行，是因为在内循环结束时没有将n1置1，将n1置1后，结果输出如下所示：

![](https://wx1.sinaimg.cn/mw690/006K22Ivly1fyl9doh5gbj30hx0dp4h6.jpg)

它的最后一行没有输出，是因为最后不够256，需要另写代码读取数据，以行读取为例，在循环后加入代码：

            //读取图像缓存
			poSrcDS1->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandR,  imgXlen, imgYlen-(m-1)*256,GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(2)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandG,  imgXlen, 256, GDT_Float32, 0, 0);
			poSrcDS1->GetRasterBand(3)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandB,  imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poSrcDS2->GetRasterBand(1)->RasterIO(GF_Read,   0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandP,  imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			//RGB与IHS间的变换
			for (i = 0; i < imgXlen * (imgYlen-(m-1)*256); i++)
			{
				bandH[i] = -sqrt(2.0f)/6.0f*bandR[i]-sqrt(2.0f)/6.0f*bandG[i]+sqrt(2.0f)/3.0f*bandB[i];
				bandS[i] = 1.0f/sqrt(2.0f)*bandR[i]-1/sqrt(2.0f)*bandG[i];

				bandR[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]+1.0f/sqrt(2.0f)*bandS[i];
				bandG[i] = bandP[i]-1.0f/sqrt(2.0f)*bandH[i]-1.0f/sqrt(2.0f)*bandS[i];
				bandB[i] = bandP[i]+sqrt(2.0f)*bandH[i];
			}

			poDstDS->GetRasterBand(1)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandR, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(2)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandG, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);
			poDstDS->GetRasterBand(3)->RasterIO(GF_Write, 0, 256*(m-1), imgXlen, imgYlen-(m-1)*256,
				bandB, imgXlen, imgYlen-(m-1)*256, GDT_Float32, 0, 0);

这样输出时时正确结果。

两种方式相比较，块读写方式较行来说，要慢一些。因为行读写方式能大大减少磁盘的I/O次数，每一块的每行数据在磁盘上都是连续的，因此在读写时只需将文件指针定位到块的起始位置，就能实现整块的读写。但256*256的块读写方式，读写完一小块就需要移动指针，使磁盘的I/O次数大大增加，所以，行读写方式较快。