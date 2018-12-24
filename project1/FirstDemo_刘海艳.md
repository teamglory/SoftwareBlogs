实验过程

1.建一个工程FirstDemo，选择win32控制台程序。

2.添加代码。将 gdal头文件和需要用到的lib文件拷到项目目录下，把gdal18.dll拷到项目的Release目录下，将图片放到目录下。

3.运行，得出正确结果。

图片：
![trees](https://wxt.sinaimg.cn/thumb300/006K22Ivly1fw17m9dfiwj30ev07mjrz.jpg?tags=%5B%5D)

结果：
![](https://wx1.sinaimg.cn/mw1024/006K22Ivly1fw17jzmssqj30ee03kjr7.jpg)



过程中遇到的问题：

1.过程中发现没有Release目录，无法将gdal的动态链接库考到目录下。

解决：运行旁边有个选项，可以选择Release或Debug，正确选择后就可以了，再将gdal的动态链接库考到目录下。

2.敲代码时，出现错误。