---
layout: post
title: RTSP协议简介
categories: [协议]
---

# RTSP协议介绍

> * RTSP（Real Time Streaming Protocol），RFC2326，实时流传输协议，是TCP/IP协议体系中的一个应用层协议，由哥伦比亚大学、网景和RealNetworks公司提交的IETF RFC标准。该协议定义了一对多应用程序如何有效地通过IP网络传送多媒体数据。RTSP在体系结构上位于RTP和RTCP之上，它使用TCP或UDP完成数据传输。
> * RTSP协议以客户服务器方式工作，它是一个多媒体播放控制协议，用来使用户在播放从因特网下载的实时数据时能够进行控制，如：暂停/继续、后退、前进等。因此 RTSP 又称为“因特网录像机遥控协议”。

### 协议特点

* 可扩展性：新方法和参数很容易加入RTSP。
* 易解析：RTSP可由标准HTTP或MIME解析器解析。
* 安全：RTSP使用网页安全机制。
* 独立于传输：RTSP可使用不可靠数据报协议（EDP）、可靠数据报协议（RDP）；如要实现应用级可靠，可使用可靠流协议。
* 多服务器支持：每个流可放在不同服务器上，用户端自动与不同服务器建立几个并发控制连接，媒体同步在传输层执行。
* 记录设备控制：协议可控制记录和回放设备。
* 流控与会议开始分离：仅要求会议初始化协议提供，或可用来创建惟一会议标识号。特殊情况下，可用SIP或H.323来邀请服务器入会。
* 适合专业应用：通过SMPTE时标，RTSP支持帧级精度，允许远程数字编辑。
* 演示描述中立：协议没强加特殊演示或元文件，可传送所用格式类型；然而，演示描述至少必须包括一个RTSP URL。
* 代理与防火墙友好：协议可由应用和传输层防火墙处理。防火墙需要理解SETUP方法，为UDP媒体流打开一个“缺口”。
* 适当的服务器控制：如用户启动一个流，必须也可以停止一个流。
* 传输协调：实际处理连续媒体流前，用户可协调传输方法。
*  性能协调：如基本特征无效，必须有一些清理机制让用户决定哪种方法没生效。这允许用户提出适合的用户界面。

##### 与HTTP协议相比的特点

* HTTP请求由客户机发出，服务器作出响应；使用RTSP时，客户机和服务器都可以发出请求，即RTSP可以是双向的。
* RTSP是用来控制声音或影像的多媒体串流协议，并允许同时多个串流需求控制，传输时所用的网络通讯协定并不在其定义的范围内，服务器端可以自行选择使用TCP或UDP来传送串流内容，它的语法和运作跟HTTP 1.1类似，但并不特别强调时间同步，所以比较能容忍网络延迟。
* HTTP友好：此处，RTSP明智地采用HTTP观念，使现在结构都可重用。结构包括Internet内容选择平台（PICS）。由于在大多数情况下控制连续媒体需要服务器状态，RTSP不仅仅向HTFP添加方法。

##### 与HTTP协议区别

* RTSP引入了几种新的方法，比如DESCRIBE、PLAY、SETUP 等，并且有不同的协议标识符，RTSP为rtsp 1.0,HTTP为http 1.1；
* HTTP是无状态的协议，而RTSP为每个会话保持状态；
* RTSP协议的客户端和服务器端都可以发送Request请求，而在HTTPF 协议中，只有客户端能发送Request请求。
* 在RTSP协议中，载荷数据一般是通过带外方式来传送的(除了交织的情况)，及通过RTP协议在不同的通道中来传送载荷数据。而HTTP协议的载荷数据都是通过带内方式传送的，比如请求的网页数据是在回应的消息体中携带的。
* 使用ISO 10646(UTF-8) 而不是ISO 8859-1，以配合当前HTML的国际化；
* RTSP使用URI请求时包含绝对URI。而由于历史原因造成的向后兼容性问题，HTTP/1.1只在请求中包含绝对路径，把主机名放入单独的标题域中；

### RTSP协议控制

> 要实现 RTSP 的控制功能，不仅要有协议，而且要有专门的媒体播放器(media player)和媒体服务器(media server)。媒体服务器与媒体播放器的关系是服务器与客户的关系。  

 ![rtsp_player_server_model]({{ site.url }}/images/posts/rtsp_player_server_model.png)

 RTSP 仅仅是使媒体播放器能控制多媒体流的传送。因此，RTSP 又称为带外协议，而多媒体流是使用 RTP 在带内传送的。

### RTSP的报文结构

* RTSP有两类报文：请求报文和响应报文。请求报文是指从客户向服务器发送请求报文，响应报文是指从服务器到客户的回答。
* 由于 RTSP 是面向正文的(text-oriented)，因此在报文中的每一个字段都是一些 ASCII 码串，因而每个字段的长度都是不确定的。
* RTSP报文由三部分组成，即开始行、首部行和实体主体。在请求报文中，开始行就是请求行，
	* RTSP请求报文的结构如图所示：
	
	 ![rtsp_baowen_zucheng]({{ site.url }}/images/posts/rtsp_baowen_zucheng.png)

*  RTSP请求报文的方法包括：OPTIONS、DESCRIBE、SETUP、TEARDOWN、PLAY、PAUSE、GET_PARAMETER和SET_PARAMETER。RTSP请求报文的常用方法及作用如表所示。

		方法                                     作用
		OPTIONS                                获得服务器提供的可用方法
		DESCRIBE                               得到会话描述信息
		SETUP                                  客户端提醒服务器建立会话，并确定传输模式
		TEARDOWN                               客户端发起关闭请求
		PLAY                                   客户端发送播放请求

	> * URI是接受方的地址,例如:rtsp://192.168.0.1/video1.3gp。
	> * RTSP版本一般都是 RTSP/1.0。每行后面的CR LF表示回车换行，需要接受端有相应的解析，最后一个消息头需要有两个CR LF
	> * 消息体是可选的，有的Request消息并不带消息体。

* 响应报文的开始行是状态行，RTSP响应报文的结构如图所示：

	![rtsp_response_baowen_zucheng]({{ site.url }}/images/posts/rtsp_response_baowen_zucheng.png)

	> * 其中RTSP版本一般都是RTSP/1.0,状态码是一个数值，用于表示Request消息的执行结果,比如200表示成功,解释是与状态码对应的文本解释.

### RTSP交互过程

> C表示RTSP客户端，S表示RTSP服务端

**OPTION**

	① C->S: OPTION request            //询问S有哪些方法可用
		eg:
			OPTIONS rtsp://192.168.20.136:5000/xxx666 RTSP/1.0
			CSeq: 1        
	  S->C: OPTION response        //S回应信息会在会在Public字段提供所有可用方法
		eg:
			RTSP/1.0 200 OK
			CSeq: 1         //每个回应消息的cseq数值和请求消息的cseq相对应
			Public: OPTIONS, DESCRIBE, SETUP, TEARDOWN, PLAY, PAUSE

**DESCRIBE**

	② C->S: DESCRIBE request      //要求得到S提供的媒体初始化描述信息
		eg:
			DESCRIBE rtsp://server.example.com/fizzle/foo RTSP/1.0
			CSeq: 312
			Accept: application/sdp, application/rtsl, application/mheg)
	  S->C: DESCRIBE response      //S回应媒体初始化描述信息，主要是sdp
		eg:
			RTSP/1.0 200 OK
			CSeq: 312
			Date: 23 Jan 1997 15:35:06 GMT
			Content-Type: application/sdp  //表示回应为SDP信息
			Content-Length: 376
			//这里为一个空行
			//以下为具体的SDP信息
			v=0 
			o=mhandley 2890844526 2890842807 IN IP4 126.16.64.4
			s=SDP Seminar
			i=A Seminar on the session description protocol
			u=http://www.cs.ucl.ac.uk/staff/M.Handley/sdp.03.ps
			e=mjh@isi.edu (Mark Handley)
			c=IN IP4 224.2.17.12/127
			t=2873397496 2873404696
			a=recvonly
			m=audio 3456 RTP/AVP 0
			m=video 2232 RTP/AVP 31
			m=whiteboard 32416 UDP WB
			a=orient:portrait

######  媒体初始化是任何基于RTSP系统的必要条件，但RTSP规范并没有规定它必须通过DESCRIBE方法完成。RTSP客户端可以通过以下方法来接收媒体描述信息：

* 通过DESCRIBE方法；
* 其它一些协议（HTTP，email附件，等）；
* 通过命令行或标准输入设备

**SETUP**

用于确定转输机制，建立RTSP会话。客户端能够发出一个SETUP请求为正在播放的媒体流改变传输参数，服务器可能同意这些参数的改变。若是不同意，它必须响应错误"455 Method Not Valid In This State"。<br/>
Request 中的Transport头字段指定了客户端可接受的数据传输参数；Response中的Transport 头字段包含了由服务器选出的传输参数。

	③ C->S: SETUP request         //设置会话属性，以及传输模式，提醒S建立会话
		eg:
			SETUP rtsp://example.com/foo/bar/baz.rm RTSP/1.0
			CSeq: 302
			Transport: RTP/AVP;unicast;client_port=4588-4589
		
	  S->C: SETUP response         //S建立会话，返回会话标识符及会话相关信息
		eg:
			RTSP/1.0 200 OK
			CSeq: 302
			Date: 23 Jan 1997 15:35:06 GMT
			Session: 47112344  //产生一个Session ID
			Transport: RTP/AVP;unicast;
			client_port=4588-4589;server_port=6256-6257

**PLAY**
> * PLAY方法告知服务器通过SETUP中指定的机制开始发送数据 。在尚未收到SETUP请求的成功应答之前，客户端不可以发出PLAY请求。
> * PLAY请求将正常播放时间（normal play time）定位到指定范围的起始处，并且传输数据流直到播放范围结束。PLAY请求可能被管道化（pipelined），即放入队列中（queued）；服务器必须将PLAY请求放到队列中有序执行。也就是说，后一个PLAY请求需要等待前一个PLAY请求完成才能得到执行。

比如，在下例中，不管到达的两个PLAY请求之间有多紧凑，服务器首先play第10到15秒，然后立即第20到25秒，最后是第30秒直到结束。

	④ C->S: PLAY request          //C请求播放
		eg:
			C->S: PLAY rtsp://audio.example.com/audio RTSP/1.0
			CSeq: 835
			Session: 12345678
			Range: npt=10-15
			 
			C->S: PLAY rtsp://audio.example.com/audio RTSP/1.0
			CSeq: 836
			Session: 12345678
			Range: npt=20-25
			 
			C->S: PLAY rtsp://audio.example.com/audio RTSP/1.0
			CSeq: 837
			Session: 12345678
			Range: npt=30-

	  S->C: PLAY response          //S回应请求信息
	  S->C: 发送流媒体数据
	
Range头可能包含一个时间参数。该参数以UTC格式指定了播放开始的时间。如果在这个指定时间后收到消息，那么播放立即开始。时间参数可能用来帮助同步从不同数据源获取的数据流。

不含Range头的PLAY请求也是合法的。它从媒体流开头开始播放，直到媒体流被暂停。如果媒体流通过PAUSE暂停，媒体流传输将在暂停点（the pause point）重新开始。

如果媒体流正在播放，那么这样一个PLAY请求将不起更多的作用，只是客户端可以用此来测试服务器是否存活。

**PAUSE**

PAUSE请求引起媒体流传输的暂时中断。如果请求URL中指定了具体的媒体流，那么只有该媒体流的播放和记录被暂停（halt）。比如，指定暂停音频，播放将会无声。如果请求URL指定了一组流，那么在该组中的所有流的传输将被暂停。如：

		C->S:   PAUSE rtsp://example.com/fizzle/foo RTSP/1.0
				CSeq: 834
				Session: 12345678
		 
		S->C:   RTSP/1.0 200 OK
				CSeq: 834
				Date: 23 Jan 1997 15:35:06 GMT

PAUSE请求中可能包含一个Range头用来指定何时媒体流暂停，我们称这个时刻为暂停点（pause point）。该头必须包含一个精确的值，而不是一个时间范围。媒体流的正常播放时间设置成暂停点。当服务器遇到在任何当前挂起（pending）的PLAY请求中指定的时间点后，暂停请求生效。如果Range头指定了一个时间超出了任何一个当前挂起的PLAY请求，将返回错误"457 Invalid Range" 。如果一个媒体单元（比如一个音频或视频禎）正好在一个暂停点开始，那么表示将不会被播放或记录。如果Range头缺失，那么在收到暂停消息后媒体流传输立即中断，并且暂停点设置成当前正常播放时间。

**TEARDOWN**

	⑤ C->S: TEARDOWN request     //C请求关闭会话
		eg:
			EARDOWN rtsp://example.com/fizzle/foo RTSP/1.0
			CSeq: 892
			Session: 12345678

	  S->C: TEARDOWN response     //S回应请求
		eg:
			RTSP/1.0 200 OK
			CSeq: 892
		


**上述的过程是标准的RTSP流程，其中第3步和第4步是必需的。**

######　一次基本的RTSP操作过程

> * 客户端连接到流服务器并发送一个RTSP描述命令（DESCRIBE）。
> * 流服务器通过一个SDP描述来进行反馈，反馈信息包括流数量、媒体类型等信息。
> * 客户端再分析该SDP描述，并为会话中的每一个流发送一个RTSP建立命令(SETUP)，RTSP建立命令告诉服务器客户端用于接收媒体数据的端口。
> * 流媒体连接建立完成后，客户端发送一个播放命令(PLAY)，服务器就开始在UDP上传送媒体流（RTP包）到客户端。
> * 在播放过程中客户端还可以向服务器发送命令来控制快进、快退和暂停等。
> * 客户端可发送一个终止命令(TERADOWN)来结束流媒体会话

### RTSP重要术语

* **集合控制(Aggregate control ):**

> 对多个流的同时控制。对音频/视频来讲，客户端仅需发送一条播放或者暂停消息就可同时控制音频流和视频流。

* **实体(Entity)：**

> 作为请求或者回应的有效负荷传输的信息。由以实体标题域（entity-header field）形式存在的元信息和以实体主体（entity body）形式存在的内容组成

* **容器文件（Container file）：**

> 可以容纳多个媒体流的文件。RTSP服务器可以为这些容器文件提供集合控制。

* **RTSP会话(RTSP session )：**

> RTSP交互的全过程。对一个电影的观看过程,会话(session)包括由客户端建立媒体流传输机制(SETUP)，使用播放(PLAY)或录制(RECORD)开始传送流，用停止(TEARDOWN)关闭流。


### RTSP重要头字段参数

1. **Accept**:

用于指定客户端可以接受的媒体描述信息类型。比如:

	Accept: application/rtsl, application/sdp;level=2

2. **Bandwidth**:

用于描述客户端可用的带宽值。

3. **CSeq**：

指定了RTSP请求回应对的序列号，在每个请求或回应中都必须包括这个头字段。对每个包含一个给定序列号的请求消息，都会有一个相同序列号的回应消息。

4. **Rang**:

用于指定一个时间范围，可以使用SMPTE、NTP或clock时间单元。

5. **Session**:

 Session头字段标识了一个RTSP会话。Session ID 是由服务器在SETUP的回应中选择的，客户端一当得到Session ID后，在以后的对Session 的操作请求消息中都要包含Session ID.

6. **Transport**:

 Transport头字段包含客户端可以接受的转输选项列表，包括传输协议，地址端口，TTL等。服务器端也通过这个头字段返回实际选择的具体选项。如:

	Transport: RTP/AVP;multicast;ttl=127;mode="PLAY",
	RTP/AVP;unicast;client_port=3456-3457;mode="PLAY"