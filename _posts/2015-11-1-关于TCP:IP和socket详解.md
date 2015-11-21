
---
layout: post
title: "关于TCP/IP还有socket详解原创"
description: ""
category: 
tags: []
---

{% include JB/setup %}



#关于TCP/IP还有socket详解（原创）

###1、熟悉TCP/UDP/HTTP？
![原理图](http://wiki.mbalib.com/w/images/a/ac/TCP%EF%BC%8FIP%E5%8D%8F%E8%AE%AE%E6%97%8F%E4%B8%AD%E5%90%84%E5%8D%8F%E8%AE%AE%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB.jpg)
详细原理请参照[这里](http://wiki.mbalib.com/wiki/TCP/IP);
TCP支持的应用协议主要有：Telnet、FTP、SMTP等；

UDP支持的应用层协议主要有：NFS（网络文件系统）、SNMP（简单网络管理协议）、DNS（主域名称系统）、TFTP（通用文件传输协议）等


答：TCP：全称是Transmission Control Protocol ，中文名位传输控制协议，它可以提供可靠的、面向连接的网络数据传递服务。传输控制协议主要包括下列任务和功能：

 1. 确保ip数据报的成功传递
 2. 对程序发送的大块数据进行分段和重组。
 3. 确保正确排序及按顺序传递分段的数据。
 4. 通过计算校验和，进行传输数据的完整性检查。
 
UDP:全称是：User Datagram Protocol又称使用者资料包协定，是一个简单的面向数据报的传输层协议
在TCP/IP模型中，UDP为网络层以上和应用层以下提供了一个简单的接口。UDP只提供数据的不可靠传递，它一旦把应用程序发给网络层的数据发送出去，就不保留数据备份（所以UDP有时候也被认为是不可靠的数据报协议）。UDP在IP数据报的头部仅仅加入了复用和数据校验（字段）。

HTTP是客户端用http协议进行请求，发送请求时候需要封装http请求头，并绑定请求的数据，服务器一般有web服务器配合（当然也非绝 对）。 http请求方式为客户端主动发起请求，服务器才能给响应，一次请求完毕后则断开连接，以节省资源。服务器不能主动给客户端响应（除非采取http长连接 技术）。iphone主要使用类是NSUrlConnection。

UDP应用场景：

1，实时音视频是可以而且应该用 UDP 的，一方面因为它常常涉及到网络穿透，另外一方面它不需要重传。——我需要实时的看到你的图像跟声音，至于中间丢一帧什么的完全不重要。而为了重传往往会造成延迟与不同步，考虑一下，某一帧因为重传，导致0.5秒以后才到，那么整个音视频就延迟了0.5秒。
著作权归作者所有。

2，网络真的非常非常可靠，以至于你完全不需要考虑 UDP 丢包问题的情况。典型的例子应该是专门为有线局域网设计的协议。

3，另外一个问题是 TCP 是纯粹的流式数据，所以制定传输协议的时候，接受方需要自行判定一个包的开始和结束，因为你完全可能接受到半个包或者两个包。——如果数据报的起止判定对你具体的程序会成为大问题，也可以考虑 UDP

when in doubt, use tcp

###什么是socket？
答：scoket是客户端跟服务器直接使用socket“套接字”进行连接，并没有规定连接后断开，所以客户端和服务器可以保持连接通道，双方 都可以主动发送数据。一般在游戏开发或股票开发这种要求即时性很强并且保持发送数据量比较大的场合使用。主要使用类是CFSocketRef。
用到socket的第三方：AsyncSocket

###网络访问的OSI七层模型：从上到下依次是：应用层、表示层、会话层、传输层 、网络层、数据链路层、物理层。
###七层简述 
.1. 物理层:主要定义物理设备标准,如网线的接口类型、各种传输介质的传 输速率等。主要作用是传输比特流(就是由1、0转化为电流强弱来进行传 输,到达目的地后再转化为1、0,也就是常说的数模与模数转换)。这一 层的数据叫做比特(bit),主要设备:集线器 

.2. 数据链路层:主要将从物理层接收的数据进行MAC地址的封装与解封装。 常把这一层的数据叫做帧,主要设备:交换机 

.3. 网络层:选择合适的网间路由和交换结点, 确保数据及时传送,将从下层 接收到的数据进行IP地址的封装与解封装。常把这一层数据叫做数据包,主 要设备:路由器。 

.4. 传输层:定义了一些传输数据的协议和端口,如TCP、UDP协议,主要将从 下层接收的数据进行分段和传输,到达目的地址后再进行重组,以往把这 一层数据叫做段。 

.5. 会话层:通过传输层建立数据传输通路。在系统之间发起会话或者接受会 话请求(设备之间需要互相认识) 

.6. 表示层:主要是进行对接收的数据进行解释、压缩与解压缩等,即把计算 机能够识别的东西转化成人能够识别的东西(如图片、声音等) 

.7. 应用层:主要是一些终端的应用,比如说FTP(各种文件下载)、浏览器、 QQ等,可以将其理解为在电脑屏幕上可以看到的东西,也就是终端应用。 
###tcp/ip四层模型：从上到下依次：应用层、传输层、网际层、主机至网络层
![网络参考模型](http://i5.tietuku.com/7df4467aa74a7720.png)

1.  网络中各节点都有相同的层次
2.  不同节点相同层次具有相同的功能
3.  同一节点相邻层间通过接口通信
4. 每一层可以使用下层提供的服务
5. 不同节点的同层间通过协议来实现对等层间的通信
6. ![插图](http://i5.tietuku.com/d8a23048330df782.png)

####网络通讯要素 
• IP地址(唯一标示网络设备的0~255 2^32 = 4G): - 网络中设备的标示- 不易记忆,可以用主机名-本地回环地址:127.0.0.1 主机名:localhost 
• 端口号(定位程序) 
- 用于标示进程的逻辑地址,不同进程的标示 
- 有效端口:0~65535,其中0~1024由系统使用或者保留端口,开发 中不要使用1024以下的端口 
• 传输协议(用什么样的方式进行交互) - 通讯的规则 
- 常见协议:TCP、UDP 
• http://ip:80/文件路径 => URL(统一资源定位) 
• 资源类型是通过MimeType来区分的,告诉客户端是什么类型的 资源
URL(确定要访问的资源) •Request=》要访问了• Connect=》开始访问• ....
• 返回结果

##TCP & UDP 
• UDP(用户数据报协议)- 将数据及源和目的封装成数据包中,不需要建立连接 - 每个数据报的大小限制在64K之内- 因为无需连接,因此是不可靠协议- 不需要建立连接,速度快

• TCP(传输控制协议)- 建立连接,形成传输数据的通道- 在连接中进行大数据传输(数据大小不收限制) - 通过三次握手完成连接,是可靠协议,安全送达 - 必须建立连接,效率会稍低 
##Socket(套接字层、插座--AT&T) 
• Socket就是为网络服务提供的一种机制 - 在Unix中,网络既是Socket,并不局限在TCP/UDP - Socket可以用于自定义协议 QQ

• 通信的两端都是Socket

• 网络通信其实就是Socket间的通信• 数据在两个Socket间通过IO传输 

原理图

![原理图](http://i12.tietuku.com/9a28f7b80752179a.png)

Socket通讯流程图 

![Socket通讯流程图](http://i5.tietuku.com/6a59569c051567db.jpg)

使用Socket开发网络通讯 
• 在Web服务(WebServices=>XML)大行其道的今天,调用Web服 务的代价是高昂的,尤其是仅仅是抓取少量数据的时候尤其如 此。而使用Socket,可以只传送数据本身而不用进行XML封装, 大大降低数据传输的开销(JSON)
• Socket允许使用长连接,允许应用程序运行在异步模式(提高 效率),只有在需要的时候才接收数据 

####iOS中常用的两种Socket类型 
流式Socket(SOCK_STREAM):流式是一种面向连接的
Socket,针对于面向连接的TCP服务应用
数据报式Socket(SOCK_DGRAM):数据报式Socket是一 种无连接的Socket,对应于无连接的UDP服务应用

在iOS中流式Socket连接的方法 
-  在iOS中以NSStream(流)来发送和接收数据• 可以设置流的代理,对流状态的变化做出相应
- 连接建立- 接收到数据 - 连接关闭

1. NSStream:数据流的父类,用于定义抽象特性,例如:打开、关闭 代理,NSStream继承自CFStream(Core Foundation)
2. NSInputStream:NSStream的子类,用于读取输入 
3. NSOutputStream:NSSTream的子类,用于写输出 

####开发步骤 

1. 网络连接设置1. 设置网络连接,绑定到主机和端口2. 设置输入流和输出流的代理,监听数据流的状态 3. 将输入输出流添加至运行循环4. 打开输入流和输出流
2. 发送消息给服务器
3. 有可读取字节时,读取服务器返回的内容
4. 到达流末尾时,关闭流,同时并从主运行循环中删除

设置网络通讯 

		
		CFReadStreamRef readStream;!
		CFWriteStreamRef writeStream;!
		!
		CFStreamCreatePairWithSocketToHost(NULL, (CFStringRef)@"localhost", 12345, &readStream, &writeStream);!
		!
		  _inputStream = (__bridge NSInputStream *)readStream;!_outputStream = (__bridge NSOutputStream*)writeStream;!
		  
		  
说明:CFStreamCreatePairWithSocketToHost函数用于将输入流和输出流 绑定到指定主机的对应端口,连接建立之后,既可以像输入流写入数据, 或者从输出流读取数据
		
		
		设置流代理并添加至运行循环 
		_inputStream.delegate = self;!
		_outputStream.delegate = self;!
		!
		// 将输入、输出流添加至运行循环!
		[_inputStream scheduleInRunLoop:[NSRunLoop mainRunLoop]forMode:NSDefaultRunLoopMode];!
		[_outputStream scheduleInRunLoop:[NSRunLoop mainRunLoop]forMode:NSDefaultRunLoopMode];!
		!
		// 打开输入、输出流 [_inputStream open];! [_outputStream open]; 
		
发送登录请求给服务器 


     // 1. 创建要发送的字符串 NSString *sendMsg = [NSStringstringWithFormat:@"iam:%@",
				_userNameText.text];!
				// 2. 将字符串转换成NSData NSData *sendData = [sendMsgdataUsingEncoding:NSUTF8StringEncoding];! !
				// 3. 写入数据 [_outputStream write:[sendData bytes]maxLength:[sendData length]]; 
		
数据流事件响应 

		switch (eventCode) {!
		case NSStreamEventOpenCompleted:!
		NSLog(@"数据流打开完成");!
		break;!
		case NSStreamEventHasBytesAvailable:!
		NSLog(@"有字节读取");!
		break;!
		case NSStreamEventHasSpaceAvailable:!
		NSLog(@"可以写入数据");!
		break;!
		case NSStreamEventErrorOccurred:!
		NSLog(@"无法连接到服务器");!
		break;!
		case NSStreamEventEndEncountered:!
		NSLog(@“到达流末尾,需要关闭流");!
		break;! default:!
		NSLog(@"未知");!
		break;! } 
		
有字节读取,则读取从服务器返回消息 

		// 服务器返回数据,从输入流中读取数据 // 定义一个字符串缓冲数组,用于接收数据 uint8_t buffer[1024];!
		// 送输入流中读取数据,并获得读取内容的长度 int len = [_inputStream read:buffermaxLength:sizeof(buffer)];! !
		// 判断是否有读入的内容 if (len > 0) {!
		// 将读入的数据转换成字符串 NSString *str = [[NSString alloc] initWithBytes:buffer
		length:len encoding:NSUTF8StringEncoding];!NSLog(@"=======> %@", str);!
		} 
		
		到达流末尾,关闭流并且从运行循环中删除 • [aStreamclose];!
		• [aStreamremoveFromRunLoop:[NSRunLoopmainRunLoop]forMode:NSDefaultRunLoopMode];!

回顾 
• Socket就是为网络服务提供的一种机制•Socket允许使用长连接,允许应用程序运行在异步模式,
只有在需要的时候才接收数据
• 流式Socket(SOCK_STREAM):流式是一种面向连接的 Socket,针对于面向连接的TCP服务应用

###QQ用到的是什么传输协议？
答：QQ登陆采用TCP协议和HTTP协议，QQ都会有一个TCP连接来保持在线状态 。你和好友之间发送消息，主要采用UDP协议，内网传文件采用了P2P技术。

总来的说：1.登陆过程，客户端client 采用TCP协议向服务器server发送信息，HTTP协议下载信息。登陆之后，会有一个TCP连接来保持在线状态。2.和好友发消息，客户端client采用UDP协议，但是需要通过服务器转发。腾讯为了确保传输消息的可靠，采用上层协议来保证可靠传输。如果消息发送失败，客户端会提示消息发送失败，并可重新发送。3.如果是在内网里面的两个客户端传文件，QQ采用的是P2P技术，不需要服务器中转。
