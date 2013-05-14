    百度文库开源实现---如何在Centos搭建文档转换​
概述：
        OpenOffice+swftools+PyODConverter+FlexPaper实现网页上展示Word、Excel、PPT、PDF等文档。原理是通过PyODConverter调用OpenOffice将文档转换成PDF文件，然后利用swftools将PDF转换成SWF文件，在网页上用FlexPaper加载swf就实现了类似百度文库的功能。

下面我们就一步步在Centos上安装文档转换服务，并将文档展现在web页面上
一、安装X window以便运行openoffice

  yum groupinstall 'X Window System' -y
一、安装openoffice将word文件转换成pdf
1、安装openoffice
  yum install openoffice.org-writer
  yum install openoffice.org-calc
  yum install openoffice.org-draw openoffice.org-impress
  
  注： 先yum list | grep openoffice查看对应的版本安装


2、运行openoffice：

    export DISPLAY=:1
    soffice -accept="socket,port=8100;urp;"&
    
3、调用转换成pdf：

 python ./pyConverter.py './input.docx' './out.pdf'
 
 PyConvert.py
 （项目地址 ：https://github.com/mirkonasato/pyodconverter）

二、安装swftools将pdf转换成swf

1、安装依赖库

 yum install zlib-devel libjpeg-devel libpng-devel freetype-devel

2、下载swftools
  wget http://www.swftools.org/swftools-2013-02-19-1826.tar.gz 
3、安装swftools
 tar -zvxf swftools-2013-02-19-1826.tar.gz
 cd swftools-2013-02-19-1826
 ./configure
 make 
 make install
4、转换pdf到swf
pdf2swf './input.pdf' -o './out.swf' -s /data/xpdf-chinese-simplified/

三、一些问题的解决
1、中文乱码的问题
将windows系统里的 C:\Windows\Fonts\simsun.ttc 上传的openoffice的 share/fonts/truetype/目录下

安装:xpdf-chinese-simplified
