# Action SPRD image sign tool

一个给紫光展锐（下文叫它展讯）设备自动签名镜像的Github工作流。**还没写完！！！**

# Introduce

据我所知，展讯现在用了两种签名方法。请根据你的SoC型号选择正确的方法签名

## AVBTOOL签名大法

> [!Caution]
> avbtool签名大法没写完，要签的话自己看[这个教程](https://www.hovatek.com/forum/thread-32664.html)和[这个教程](https://www.hovatek.com/forum/thread-32674.html)

Avbtool，顾名思义，就是处理avb2.0（或者avb1.0也行？）镜像用的。avbtool签名大法就是用avbtool给你的镜像签名。可以看[这个](https://www.hovatek.com/forum/thread-32664.html)和[这个](https://www.hovatek.com/forum/thread-32674.html)教程来了解签名的原理。

打个比方，**SC9832E/SL8541E**就得用这个方法签名，因为它采用了Android启动时验证2.0（就是avb2.0）来验证镜像。

如果我没猜错的话，你这设备有vbmeta分区而且不是空的，那十有八九得用这方法了。

如果想进一步确认的话，可以检查下boot和vbmeta的文件头。boot的文件头跟正常Android没啥区别（前8个字节是`ANDROID!`），但是vbmeta就被爆改了（前四个字节是`DHTB`），avbtool就读不了vbmeta了。实际上，真正的文件头被后移了512字节。

## sprd_sign签名大法

或者说，额，Legacy签名大法？我觉得“sprd_sign签名大法”更贴切一点。这种方法似乎是用在老SoC上的。展讯有他们自己的签名工具。

举个栗子啊，**SC9820E/SL8521E**就用了这种办法。可能是因为大多数9820e都用了安卓4.4，所以用不到那么先进的avb2.0，但是还是有些9820e 8.1的啊（

如果我没猜错的话，你这设备没vbmeta分区，或者vbmeta分区是空的，那十有八九得用这方法了。

想进一步确认的话，可以检查下boot的文件头。boot的文件头跟上文vbmeta一样被爆改成`DHTB`了，那些boot解包工具找不到`ANDROID!`文件头就炸了（但是magisk为啥正常啊）。实际上，`ANDROID!`也被后移了512字节。vbmeta就没必要看了吧，都没有了看个几把（
