#  Wire Shark抓包

### 下载教程及参考

下载包：https://www.123pan.com/s/vaxhjv-PwVHh.html提取码:OCtL

官网：[Wireshark · Download](https://www.wireshark.org/download.html)

下载教程参考：[【2023最新版】超详细Wireshark安装保姆级教程，及简单使用Wireshark抓包(网络分析)_wireshark安装教程-CSDN博客

[](https://blog.csdn.net/Eoning/article/details/132141665)



### 使用方法

打开wires hark后想知道自己要抓哪个网段要执行以下操作

1，先win+r打开win系统终端后，输入cmd，进到终端输入ipconfig

2，找到自己上网的一个网络连接

![我用的上网的是](D:\图片\Snipaste_2024-04-20_14-31-37.png)

###### 选择上网模式抓包

![选择上网模式进行抓包](D:\图片\Snipaste_2024-04-20_14-34-52.png)

###### 看是否在进行抓包

![](D:\图片\Snipaste_2024-04-20_14-36-35.png)



### 混杂模式和普通模式

###### 混杂模式：（方便网络安全人员进行排查）

不管是什么数据，你访问啥网站，正不正确的数据等都好，他都会经过你的电脑网卡的数据都会被抓到



###### 普通模式：

只接收正确的发到你电脑网卡的数据，其他的包都抓不到



###### 怎么看是不是混杂还是普通：

![](D:\图片\Snipaste_2024-04-20_14-40-30.png)



### WireShark过滤器使用

可以过滤你想要的数据

###### Source/Destination：

![](D:\图片\Snipaste_2024-04-20_16-00-11.png)

###### 重新开启抓包：

![](D:\图片\Snipaste_2024-04-20_15-09-50.png)





###### 开始了一段时间后暂停，然后用过滤器找到你想要的数据包：



![](D:\图片\Snipaste_2024-04-20_15-14-07.png)

![](D:\图片\Snipaste_2024-04-20_15-16-56.png)

###### 指定ip寻找过滤

```css
ip.src_host == # 你自己的IP地址  		# 这个表示的是指定你自己的ip	src为Source缩写
or 或 and
ip.dst_host == # 目标的IP				# 表示你要指定的目标方IP 	 	dst为Destination缩写
```

![image-20240420160732437](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420160732437.png)



![](D:\图片\Snipaste_2024-04-20_16-13-33.png)

###### 只要是你的输入的ip，不管是目标方还是发送方都显示出来：

```css
ip.addr == # 你输入你IP
```



![image-20240420161758817](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420161758817.png)



### 用tcp过滤找出三次握手



```
SYN(synchronize)			# 指请求同步
ACK			# 指确认同步
seq 是序列号和ack配合使用，seq是一个随机生成的序列号
```

![image-20240420194201644](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420194201644.png)

```css
tcp.flags.syn ==1		#	发送对服务器的请求
tcp.flags.ack == 0		#	建立与服务器链接的确认信息
tcp.flags.ack == 1		#	ack== 1为确认关系，这个必须确认才有效
```



<img src="C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420153645465.png" alt="image-20240420153645465" style="zoom:50%;" />







###### 过滤第一次拨过目标的ip

```css
tcp.flags.ack == 0 and tcp.flags.syn == 1		# 表示过滤出第一次我的电脑ip发送请求到目标ip
```



![](D:\图片\Snipaste_2024-04-20_15-45-31.png)

###### 用过滤器找到链接建立完成后发送数据已经完成：

```css
tcp.flags.fin == 1		# 等等 1表示已经完成的数据包，fin为finish缩写
```

![image-20240420155324765](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420155324765.png)



### tcp三次握手抓云服务器示例：

这里完好的抓出了三次握手

![image-20240420213402005](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420213402005.png)





### tcp的连开连接的四次挥手：







### 常见协议包

###### 1，ARP协议

真正的上网地址是物理地址，而不是你的ip地址

arp欺骗原理具体参考视频：https://v.douyin.com/iYsQcCDc/



###### 2，ICMP协议

一般为ping的请求，

![](D:\图片\Snipaste_2024-04-20_16-29-05.png)



![image-20240420164304309](C:\Users\MAH\AppData\Roaming\Typora\typora-user-images\image-20240420164304309.png)



###### 3，TCP协议

###### 4，DNS协议

###### 5，HTTP协议