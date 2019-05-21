# 计算机网络

## 目录

<!-- MarkdownTOC -->
[计算机网络](#计算机网络)
- [目录](#目录)   
[一. 网络协议的了解](#一-网络协议的了解)      
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.OSI七层协议的概念模型了解](#1osi七层协议的概念模型了解)<BR>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;物理层](#-物理层)           
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据链路层](#-数据链路层)           
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;网路层](#-网路层)           
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;传输层](#-传输层)          
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;会话层](#-会话层)          
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;表示层](#-表示层)          
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;应用层](#-应用层)      
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.TCP/IP四层协议的标准了解](#2tcpip四层协议的标准了解)          
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1. 层级划分比较](#21-层级划分比较)<br>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2. 数据的处理流程](#22-数据的处理流程)  
 [二. TCP](#二-tcp)        
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.TCP协议简介](#1tcp协议简介)     
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.TCP报头详解](#2tcp报头详解)    
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. TCP三次握手](#3-tcp三次握手)          
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1 过程分析（重点）：](#31-过程分析重点)      
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.2 为什么要三次握手（重点）：](#32-为什么要三次握手重点)                  
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.3 为什么要传回 SYN：](#33-为什么要传回-syn)           
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.4 传了 SYN,为啥还要传 ACK：](#34-传了-syn为啥还要传-ack)<BR>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.5 为什么TCP客户端最后还要发送一次确认：](#35-为什么tcp客户端最后还要发送一次确认)           
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.6 三次握手失败情况（重点）：](#36-三次握手失败情况重点)<BR> 
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. TCP四次挥手](#emsp4-tcp四次挥手)          
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 过程分析（重点）：](#41-过程分析重点)        
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 为什么有TIME-WAIT状态（重点）：](#42-为什么有time-wait状态重点)         
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3 为什么是四次挥手（重点）：](#43-为什么是四次挥手重点)<BR> 
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4 大量报文处于CLOSE-WAIT状态原因（重点）：](#44-大量报文处于close-wait状态原因重点)      
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6. TCP与UDP的区别（重点）](6-tcp与udp的区别重点)          
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.1 UDP报文结构：](#61-udp报文结构)     
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.2 UDP与TCP的比较](#62-udp与tcp的比较)             
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.1.1粘包问题](#611-粘包问题)             
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.1.2粘包的解决办法](#612-粘包的解决办法)

  [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;三. HTTP](#三-http)        
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 请求结构](#1-请求结构)        
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2 响应结构](#2-响应结构)         
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3 请求响应步骤](#3-请求响应步骤)          
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4 浏览器输入URL，按下回车之后会经历什么流程（重要）](#4-浏览器输入url按下回车之后会经历什么流程重要)         
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5 状态码(重要)](#5-状态码重要)     
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6 GET与POST请求区别(重要)](#6-get与post请求区别重要)[UNDO  需要补充](#undo--需要补充)         
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7 cookie与session(重要)](#7-cookie与session重要)                    
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.1 cookie流程图](#71-cookie流程图)            
  [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.2 cookie流程详解](#72-cookie流程详解)                
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.3 session实现方式](#73-session实现方式)             
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.4 Tomcat使用session方式](#74-tomcat使用session方式)<BR>
  [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.5 cookie与session的区别（重要）](#75-cookie与session的区别重要)          
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8 HTTP与HTTPS的区别(重要)](#8-http与https的区别重要)
 <br>[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.1 结构对比](#81-结构对比)            
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.2 SSL简介](#82-ssl简介)        
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.3 几种加密方式](#83-几种加密方式)<br>
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.4 https数据传输流程](#84-https数据传输流程)            
 [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.5 https与http区别](#85-https与http区别)
  <br>[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.6 https真的安全吗](#86-https真的安全吗)        
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9. Socket](#9-socket)  
[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.1 Socket简介](#91-socket简介)
<BR>[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.2 Socket通信流程](#92-socket通信流程)
<!-- /TOC -->

<!-- /MarkdownTOC -->


    






## 一. 网络协议的了解
   市面上现在存在4层 5层 7层协议，OSI7层模型是业界的概念框架。


### 1.OSI七层协议的概念模型了解
  
 
![](https://s2.ax1x.com/2019/05/20/Ev1xOK.png)![](https://s2.ax1x.com/2019/05/20/Ev1jQx.png)

#### -物理层


物理层主要定义了物理设备的标准，如网线的类型，光纤接口的类型，各种传输介质的速率。
他的主要任务是传输比特流，即0101二进制数据将其转换为电流强弱，传输到目标机器上。然后再由目标机器的物理层将其转换为0101数据的传输（即数模转换 模数转换的过程），这层的传输形式是**BIT**，**网卡**就是工作在这层。

#### -数据链路层


传输BIT的时候会出现错传或者传输不完整的问题，因此数据链路层定义了**如何格式化的数据**来进行传输和**控制对物理介质的访问**以及**错误检测和纠正**。这层传输形式是**帧**，**交换机**就是工作在这层。

#### -网路层
网络层主要功能是将网络地址翻译成物理地址，并将其路由到接收方。**路由器**属性网络层，此层的数据为**网络包。**
#### -传输层
他解决了数据传输与数据质量问题，它规定了发送速率。对发送数据进行了切割以及标注传输数据号使接收端数据顺序正常。
#### -会话层
建立管理和应用程序之间的通讯。
#### -表示层
解决不同系统之间通讯语法的问题。
#### -应用层
规定发送接收方发送消息的格式，以方便来进行数据的解析。如果该层，虽然可以数据传输，但是传输的数据只是01101等没有实际意义。

<br>

### 2.TCP/IP四层协议的标准了解
OSI只是一个概念上的模型，实际上作为标准应用的是TP/IP四层协议

####  2.1. 层级划分比较

![](https://s2.ax1x.com/2019/05/20/Ev1XS1.png)



###  2.2. 数据的处理流程

![](https://s2.ax1x.com/2019/05/20/Ev1vy6.png)

## 二. TCP

 首先要交代一下TCP/IP协议，IP协议是无连接的通信协议。，他不会占用两条正在通信的两个计算机线路之间的通信线路，IP就降低了线路需求，每条线可以同时满足许多不同计算机通信需要。通过IP消息或者其他数据会被分割为较小的包，IP负责将数据路由到目的地。但是他没有确实数据包是否是按顺序发送或者是否被破坏，所以就需要上层协议来做出控制，即传输控制协议。
###  1.TCP协议简介
-面向连接的 可靠的 基于字节流的传输层通信协议

-将应用层的数据流分割为报文段并发送给目标节点的TCP层

-数据包都有序号，对方收到则发送ACK确认，未收到就重传

-使用检验和来检验数据在传输过程中是否有误（发送和接收之前都会检验）


###  2.TCP报头详解
  

![](  https://s2.ax1x.com/2019/05/20/Ev8wb8.png)
TCP的报头部分为20字节固定长度，具体的结构各组成结构如图。

**2.1源端口:** 2个字节
> 表示发送端进程端口号

**2.2目的端口：** 2个字节
> 表示发送端进程端口号

**2.3序号：** 4个字节
> Seq number 表示我当前发送的报文的序号为多少，初始序号是由三次握手是相互规定的，假如我规定以0位开始序号seq=0，发送之后我第二次发送了100个数据，序号区间为0-99，那么如果我第三次再发送数据，开始序号就是100。<br>

**2.4确认号码:** 4个字节
> Ack number 表示我希望下次收到的报文的序号为多少，正如上面序号介绍的，第二次发送了100个数据区 seq=0间为0-99，那么下次的ACK为100。假如第三次我发送了10个数据，接收区间就是100-109，我下一次期望收到的序号 就是109+1=110，同时也表示我已经收到了前109个数据。 序号与确认序号之间存在： **Seq+发送数据长度+1 =Ack的关系**<br>

**2.5数据偏移：** 4位


> 由于TCP报文还有选项与填充，他的长度是不确定的。数据偏移表示数据段距离报头段开头多远即报头长度，4位最大1111=15 所以TCP最大为15*4=60字节。TCP头部字节范围为20-60，当其为20时，数据长度最大为65495字节。<br>


**2.6保留位：** 6位


> 用于以后扩展。

**2.7标志位：** 6位
 
 >  **-URG:**表示紧急指针有效，正常TCP是按照排队顺序发送的。URG表示该段需要紧急发送，就将该数据插入到头部，然后根据紧急指针中显示的数据长度进行紧急发送，**即使窗口为零时也可发送紧急数据 **。
> 
>   **-ACK:**确认字段，ACK=1时确认号字段有效，建立连接后每次发送数据报ACK都为1
>   
>   **-PSH:**紧急推送，表示应用层要求尽快解析该数据包。TCP收到该包后马上向上发送，而不是等到缓存满了再发送
> 
>   **-RST:** 表示复位，出现严重差错需关闭连接再重新连接时候置为1，也可以用来拒绝非法报文段和拒绝一个连接。如一个IP在企业的访问黑名单，他进行访问，直接回复RST。
>  
>   **-SYN:** 连接建立的时候用来同步信号，当SYN=1而ACK=0时，表明这是一个连接请求报文段。对方若同意建立连接，则应在相应的报文段中使用SYN=1和ACK=1。因此，SYN置为1就表示这是一个连接请求或连接接受报文。
> 
>   **-FIN:** 释放一个连接。当FIN=1时，表明此报文段的发送方的数据已发送完毕，并要求释放运输连接。
 
**2.8窗口：** 2个字节 


> 表示当前报文段方的接收窗口的缓存大小，以此来达到发送端的速率控制。

**2.10检验和：** 2个字节   [检验方法](https://www.cnblogs.com/zxiner/p/7203192.html )

**2.11紧急指针：** 2个字节 


> 他指出紧急数据的长度大小。URG=1时候才有效 

<br>

### 3. TCP三次握手
握手是为了建立连接，建立连接之后会在两个计算机之间建立一个全双工的通信。他将占用两个计算机的通信线路，直到一个或者两个计算机关闭连接。


 ![](https://s2.ax1x.com/2019/05/20/Evgnx0.jpg)


#### 3.1 过程分析（重点）：
1. 假设客户端主动打开 从CLOSED状态开始发送**SYN=1（请求连接信号）seq=x(序号初始值) **并转换为SYN-SENT状态
2. 服务器在LISTERN状态中收到SYN然后回馈 **SYN=1 ACK=1 seq =y（发送序号为y） ack=x+1(表示x序号已确认，下次发送序号用X+1)** 并转换为SVN-RCVD状态
3. 客户端收到返回的信息后状态转换为ESTAB-LISHED状态再返回**ACK =1 seq =x+1 ack=y+1**
4. 服务器收到后进入ESTAB-LISHED状态 
5. 连接建立结束 两者开始数据传送 
> **SYN  ACK/SYN都不可以附带数据 但是要消耗一个序号 ACK可以带数据 不带数据不消耗序号**
#### 3.2 为什么要三次握手（重点）：
    	 
>   为了实现数据的可靠传输，初始化C/S双方Sequenece Number的值，进而保证传输的数据顺序正常
>   
>   三次传输保证了C/S双发发送的seq初始值都被对方成功接收

#### 3.3 为什么要传回 SYN：
> syn是建立连接时候的握手信号，传回SYN表示我收到的信息就是你发送的信息

#### 3.4 传了 SYN,为啥还要传 ACK：
> 可靠传输必须保证通信双发都连接无误，SYN传递只是保证了发送方的无误，ACK回传可以保证接收方接传也没问题
#### 3.5 为什么TCP客户端最后还要发送一次确认：
> 是为了防止已经失效的报文又传到服务器，假设一个连接只是网上滞留了过一段时间又传到服务器。如只是两次连接，服务器就会为其建立连接。但是如果是三次握手，服务器发送连接后，客户端不会返回第三次握手，还是不会连接成功。





#### 3.6 三次握手失败情况（重点）：
> 
> 第一种： 客户端发送SYN之后就掉线了，服务器无法正常送达SYN/ACK信息 会导致服务器不断重新发送
>        LINUX默认重复发送5次 依次时间间隔为1 2 4 8 16 一共31S 第五次还需要等待32S才会判定超时即64秒才会断开连接，容易出现风险（SYN Flood攻击)。<BR>
>LINUX解决措施：<br>
>      SYN队列满后，TCP通过源地址端口 目标地址端口以及时间戳打造一个SYN Cookie回发给客户端
>      若是攻击者是不会回发的，正常连接会回发回来。然后通过Cookie建立连接，即使他不在连接队列中。


> 第二种: 建立连接之后，客户端掉线 


> 解决措施： 保活机制
>       一段时间，连接属于非活动状态。开启保活机制的一端向对方发送保活探测报文，如果在保活时间间隔内没收到回复，继续发送探测。多次探测达到探测最大数后，认为连接不可到达，中断。


<br>

###  4. TCP四次挥手
   挥手是为了断开连接，流程示意图


![](https://s2.ax1x.com/2019/05/20/EvHDwn.png)
#### 4.1 过程分析（重点）：
1. 在C/S两端都在连接的状态下，假设客户端申请主动关闭。他发送** seq=x FIN=1**给服务器 然后进入到**FINSH-WAIT1**状态。
2. 服务器收到报文，然后返回**ACK=1 FIN=1 ack =x+1 seq =y **然后进入**close-wait**状态（半关闭状态）
3. 客户端收到该报文后进入**FIN-WAIT2**状态 ，在此状态客户端不可以发送内容，但是可以接受内容。
4. 在半关闭状态结束后，服务器发送释放信号 **FIN=1 seq =w ack =x+1** 进入**LAST -ACK**状态
5. 客户端发送** ACK =1 seq=x+1 ack =w+1** 然后客户端进入**TIME -WAIT**状态， 服务器进入CLOSED状态
6. 客户端等待**2MSL**时间后进入**CLOSED**状态 MSL 最长报文段寿命  WIN 2分钟  LINUX 30秒

#### 4.2 为什么有TIME-WAIT状态（重点）：
   
>  ① 用来保证有足够的时间让对端收到ACK 如果被动端没有收到ACK就会重发FIN/ACK 一来一回是2MSL 如果不这样的话对端不会进入closed状态一直消耗资源
>  
>  ② 足够的时间让可能延时到达的报文都失效，不会与新的连接的报文混到一起

#### 4.3 为什么是四次挥手（重点）：
> 因为连接是全双工的，发送方与接收方都需要FIN报文和ACK报文。
> 主动申请关闭的发起FIN=1（第一次挥手）接受端收到后知道他不会发送了，但是他可能还要发送东西，所以只是返回ACK=1（第二次挥手）。当真正不需要发送之后发起FIN=1（第三次挥手），然后再回复ACK=1（第四次挥手）
>握手比他要少一次就是因为第二次ACK/SYN一起回复了，而挥手FIN与ACK分开回复。
#### 4.4 大量报文处于CLOSE-WAIT状态原因（重点）：
> 因为对方关闭了SOCKET连接，我方忙着读或者写 没有及时关闭连接。
> 
> 原因可能是代码不够规范，没有进行相关连接的关闭

<br>

###  5. TCP的滑窗

#### 5.1 RTT与RTO
>  RTT:  发送一个数据包到收到对应的ACK，所花费的时间
>  
>  RTO： 重传时间间隔（发送一个数据包，如果该时间间隔没有收到的回复就重新发送）

#### 5.2 TCP的滑动窗口
> 作用：
> 保证TCP的可靠性
> 保证TCP的流控特性


# UNDO
----------




 
###  6. TCP与UDP的区别（重点）

#### 6.1 UDP报文结构：

![](https://s2.ax1x.com/2019/05/20/ExK9LF.png)
 
**源端口：**2个字节
> 表示发送端进程端口号

**目的端口：**2个字节
> 表示发送端进程端口号

**数据报长度：**2个字节
>UDP报文（首部和数据）的总长度。最小8 bytes，只有首部，没有数据。最大值为65535 bytes。实际上，由于IPv4分组的最大数据长度为（65535 - 20 = 65515）bytes，UDP的报文长度不超过65515 bytes。IPv6允许UDP的长度超过65535，此时length字段设为0。


**16位检验和：**2个字节
#### 6.2 UDP与TCP的比较
![]( https://s2.ax1x.com/2019/05/20/ExMqCn.png)

> 首先对他们支持的目标主机做出解释，TCP是需要事先建立连接的，所以他只能一对一。UDP是无连接的，这就使得他可以一对一也可以一对多。
> 
> 其次探讨一下消息边界，UDP是面向报文的协议，应用程序发来多大的报文就发送多大的报文。如果太长就需要IP层分片。因此UDP是完全按照应用层发送来的报文进行传送，有严格的边界。
> 但是对于TCP来说他是面向流的，他只是把应用层开成一串字节流，他自己有一个缓冲，应用程序太长，TCP会把他划短一点。太短的话，他会让他再缓冲等其他的流传送一阵再一起发送。
> 
 
##### 6.2.1 粘包问题
由于TCP面向流的原因，他就可能会造成粘包问题

正常发送包是这样的

![]( https://s2.ax1x.com/2019/05/20/Ex1Mct.png)

但是也会出现下面接收到是一个报文但实际是两个报文组成的，这样就很难处理了。

![]( https://s2.ax1x.com/2019/05/20/Ex1QjP.png)

##### 6.2.2 粘包的解决办法
> 
> 粘包会出现发送粘包和接收粘包问题
> 发送粘包的话：可以强制数据立即传送的操作，不必须等到缓冲满了再发

>接收粘包的话：可以领上层应用设置合理的结构，比如报文分为报文头与报文数据，报文头标注表文长度，根据长度来进行拆包，也可以在报文末尾增加换行符表示结束。


##  三. HTTP

#### 1 请求结构
![](https://s2.ax1x.com/2019/05/20/ExoOKJ.png)
>请求数据只有在请求方法为post才有 ，不论请求数据有没有值，都要回车换行，表示结束头信息结构。<BR>

<BR>

#### 2 响应结构
![](https://s2.ax1x.com/2019/05/20/Exoqv4.png)
<BR>

<BR>

#### 3 请求响应步骤
1. 客户端连接到WEB服务器（默认端口为80）建立一个TCP套接字连接
2. 客户端发送HTTP请求
3. 服务器接收请求并返回HTTP响应
4. 释放TCP连接 如果连接方式为close 服务器主动关闭连接 如果是keep alive会保持一段时间
5. 客户端浏览器解析HTML内容（先解析状态行 查看请求是否成功然后解析每一个响应头获得字节数。获得HTML文档然后对其格式化并显示）
<BR>

#### 4 浏览器输入URL，按下回车之后会经历什么流程（重要）
1. DNS解析：浏览器依据URL逐层查询DNS服务器缓存，解析URL中的域名对应的IP。 DNS缓存查询地址依次为 浏览器缓存---系统缓存---路由器缓存---IPS服务器缓存 -----域名服务器缓存 ----顶级域名服务器缓存
2. 根据ip地址与端口建立TCP连接 
3. 发送HTTP请求
4. 服务器处理请求并返回HTTP报文
5. 浏览器解析渲染页面
6. 连接结束  
> 第五步与第六步可以认为同时发生


<BR>

#### 5 状态码(重要)
![](https://s2.ax1x.com/2019/05/20/ExH9je.png)


> 200 正常返回信息
> 
> 400 Bad Request 客户端请求语法有问题，不能被计算机理解
> 
> 401 Unauthorized 请求未经授权，该状态码必须与WWW-Authenticate报头域一起使用
> 
> 403 Forbidden 服务器收到请求 但是拒绝服务 （如一段时间访问次数太多 ，或者不让写入文件，但是用户执行了写入或者IP被静）
> 
> 404 Not Found  请求资源不存在 （如请求路径写错了）
>
> 500 Internal Server Error 服务器发生不可预期的错误（出错检查日志看哪的代码写错了）
> 
> 503 Server Unavailable  服务器当前不能处理请求 （如连接池满了，当前不能处理了）

<BR>

#### 6 GET与POST请求区别(重要)


<br>


> 
>       Http报文层面 ： 
>            GET请求信息放置在URL（键值对形式存储，请求信息之间？隔开） 
>            POST请求信息放置在报文体 所以安全性高一些（但实际不会很高，信息放置在请求头中，以明文的形式，抓包就可以得到），安全靠HTTPS保障。
>           
>       数据库层面：
>            GET符合幂等性和安全性,幂等性是对数据库一次多次操作结果一致，安全性是不改变数据库数据，  GET一般用来查询，大致认为符合。
>            POST两者都不符合，他会向数据提交数据，POST每次获得结构也有可能不一样
>            
>       其他层面：
>            GET可以被缓存，被存储 
>            POST不行   
>          每天的请求巨大，大多数都是只读。GET幂等 安全所以就缓存 不用服务器处理
>   
>             
# UNDO  需要补充#

<br>

#### 7 cookie与session(重要)

<BR>

##### 7.1 cookie流程图

![](https://s2.ax1x.com/2019/05/21/EzQ360.png)
<BR>

##### 7.2 cookie流程详解

> 
> 第一步：用户向用户发送请求 并且会提交个人信息
> 
> 第二步：浏览器对其响应，并将发来的信息回传（在响应头中而不是响应体）
> 
> 第三步：客户端再次请求，会在请求头对Cookie回发
> 
> 第四步：服务器接收后，会解析Cookie生成与客户端相对应的内容


<BR>

##### 7.3 session实现方式


> 第一种：使用Cookie 在Cookie头中添加JSEESIONID
> 
> 第二种：URL回写 
> 

<BR>

##### 7.4 Tomcat使用session方式

> 
> 同时使用cookie与url技术，如果客户端支持cookie就一直使用cookie停止url，否则反之。
> 


<BR>

##### 7.5 cookie与session的区别（重要）

> 
>   存放位置不同：cookie存放在浏览器 session存放在服务器
> 
>   安全性：session安全性高于cookie 因为cookie存在cookie欺骗 
> 
>   服务器性能：session一段时间保存在服务器上，访问增多，服务器性能会降低



*cookie欺骗： 由于cookie存放在本地，所以可以用一些手段找到其他用户的cookie直接使用或者修改供自己使用* 

#### 8 HTTP与HTTPS的区别(重要)

##### 8.1 结构对比

![](https://s2.ax1x.com/2019/05/21/EzliEF.png)


简单的说：https就是安全版的http

##### 8.2 SSL简介

> 
> 他是为网络通信安全与数据完整性的一个协议，位于TCP与各应用层之间，是操作系统提供的API，SLL 3.0之后更名为TLS
> 
> 他采用的是身份验证与数据加密来保证网络通信的安全与数据的完整性 他在HTTPS中用来验证证书

<BR>

##### 8.3 几种加密方式

> 对称加密：加密与解密都使用同一个秘钥 （性能高）
> 
> 非对称加密： 加密的秘钥与解密的秘钥不同  （安全度高，加密有限）
> 
> hash算法：将任意长度的信息转换为固定长度的值 算法不可逆（常见的有MD5）
> 
> 数字签名：证明某个信息或文件是某人发出的 

<BR>

##### 8.4 https数据传输流程
**HTTPS是证书配合各种加密的方式进行加密传输**
<BR>

![](https://s2.ax1x.com/2019/05/21/Ezl2rV.png)
<bR>
> 
>   第一步：客户端请求服务器 并将本地支持的加密方式与一个随机数告诉服务器
>   
>   第二步：服务器选择一套浏览器支持的加密算法以及另一个随机数 ，以证书形式（包含证书公钥，发布者，签名等等）回发给浏览器
> 
>   第三步：浏览器判断证书是否有效，如果无效弹出警告，有效的话生成一个随机数用证书的公钥加密并发送给服务器
> 
>   第四步：服务器用私钥解密解密然后将三个随机数与秘钥结合形成新的秘钥 此时服务器也根据加密算法与这三个数生成了同样的秘钥
> 
>   第五步：双方通过这个秘钥进行传输内容 （对称加密 ）


*值得注意的是HTTPS一般只有在第一次握手的时候才会非对称加密，其余交互都是对称加密*

<BR>

##### 8.5 https与http区别

> 
> HTTPS需要导CA申请证书    HTTP不需要
> 
> HTTPS密文传输  HTTP明文传输
> 
> 连接方式不用 HTTPS默认使用443端口 HTTP默认使用80端口
> 
> HTTP连接较为简单 是无状态的， HTTPS是http+ssl构成的加密验证保护 他是有状态的
> <BR>

##### 8.6 https真的安全吗

> 
> 我们访问的时候只会写域名，而不添加http://或者https:// 后台默认通过http跳转到https，这样就存在劫持隐患
> 
> 比如有的网站开启了https，但为了照顾用户的使用体验（因为用户总是很赖的，一般不会主动键入https，而是直接输入域名, 直接输入域名访问，默认就是http访问）同时也支持http访问，当用户http访问的时候，就会返回给用户一个302重定向，重定向到https的地址，然后后续的访问都使用https传输,这种通信模式看起来貌似没有问题，但细致分析，就会发现种通信模式也存在一个风险，那就是这个302重定向可能会被劫持篡改，如果被改成一个恶意的或者钓鱼的https站点，然后，你懂得，一旦落入钓鱼站点 

[参考来自 ](https://www.chinaz.com/program/2015/0511/405146.shtml )

> 出现这样情况就要引入HSTS 强制要求访问都为HTTPS

###  9. Socket

*本地主机可以使用PID唯一表一个进程，但是网络中PID冲突的可能性很高，网络中使用 IP +协议+端口 唯一标识网络中的一个进程，标识之后就可以使用socket进行通信*

#### 9.1 Socket简介

> Socket是对TCP/IP协议的抽象，是操作系统对外开放的接口
> 



#### 9.2 Socket通信流程
![](https://s2.ax1x.com/2019/05/21/EzGnRH.png)


 









