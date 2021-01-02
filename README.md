# 图片隐写术

## 前置知识

在计算机中图片是由若干像素点构成的，每一个像素点都对应一个RGBA值，如果将某张图片的若干像素点的RGBA值进行+1或-1的操作得到新的图片，与原有图片相比较，人眼难以分辨出区别，利用这一点，我们可以将信息储藏在图片中，成为“图片隐写术”。

## 我的具体实现

1. 获取用户输入信息，比如：“你好”。
2. `str.charCodeAt(i)`将输入信息转为`UTF-16` 编码下的值[0, 65536)。“你”(20320)“好”(22909)。
3. 十进制转16进制(凑够4位，不足0补) “你”(`20320 -> 4f60`)“好”(`22909 -> 597d`)。
4. 16进制转2进制 “你”(`4f60 -> 0100 1111 0110 0000`)。
5. 信息存储：每个像素修改RGBA(+1或-1)可储存4位信息，比如`rgba(r,g,b,a)->rgba(r,g+1,b,a)`表示存储0100，则“你”这个字需要4个像素值存储。其实任何一个汉字和字符都是需要四个像素值存储。
6. 对应像素值修改后保存新图。
7. 解码是编码的逆过程。

## Bugs

1. 不知为什么，我用某些图片测试(如 uniapp logo)的时候，前几个像素值甚至出现了G+3的错误，我避开了对前四个像素值编码，从第五个像素值开始存储信息。

2. 不知为什么，对于不同的需要存储的信息，不同的图片会出现不同的少量错误。

   > 储存信息：1234567890123456789012345678901234567890

   ![before1](E:\FrontEnd\command_line_tools\Image_steganography\images\encode.png)

   > 出现错误

   ![after1](E:\FrontEnd\command_line_tools\Image_steganography\images\after.png)

   > 储存同样的信息：

   ![before2](E:\FrontEnd\command_line_tools\Image_steganography\images\before2.png)

   > 出现的错误不同：

   ![after2](E:\FrontEnd\command_line_tools\Image_steganography\images\after2.png)

   