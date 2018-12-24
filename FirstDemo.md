# SoftwareBlogs  
实验遇到的问题：    
第一个实验的代码和实验的详细步骤老师已经给出了，我将老师给出的代码添加在firstDemo.cpp中，然后试着运行一下，发现有错误。  
然后我把需要的lib文件拷贝到项目目录下。  
并且在#include "./gdal/gdal_priv.h"的下方添加#pragma comment(lib, "gdal_i.lib")
再次尝试，发现这个时候程序可以执行，但是会报错。  
然后我将gdal18.dll拷到项目的Release目录下，程序就可以运行了。  
![ ](https://wx4.sinaimg.cn/mw690/006BVWLMly1fwyp2rfimjj30nq0bg0t8.jpg)  
感想：通过这次实验让我对gdal库有了一些了解，还在写blog过程中体会到markdown的方便和专业化。这次试验也激发了我对图片处理的兴趣。
