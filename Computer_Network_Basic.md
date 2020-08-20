1. 当今的因特网， 两种最著名的分组交换机（packet switch）是路由器（router）和链路层交换机（link-layer switch）

2. 端系统通过ISP(Internet Service Provider) 接入因特网。例如本地电缆或电话公司那样的住宅区ISP, 公司ISP, 大学ISP，以及机场，旅馆，咖啡店，和其他公共场所提供的WIFI接入的ISP  

3. 每个ISP是一个， 由多个分子交换机和多段通信链路组成的网络。

4. 各ISP为端系统提供了各种不同类型的网络接入， 例如线缆调制解调器，DSL那样的住宅宽带接入，高速局域网接入，无线接入，56kbps拨号调制解调器接入

5. 底层的ISP通过国家国际的高层ISP连接，高层的ISP由通过高速光纤链路互联的高速路由器组成。无论是高层还是底层ISP网络，它们每个都是独立管理，运行着IP协议

6. 网络边缘 ===>  应用程序和端系统

7. 接入网（access network） ====> 端系统连接到边缘路由器（edge router） 的物理链路

8. 边缘路由器是端系统连接到任何其它远程端系统的路径上的第一个路由器

9. 用户从提供本地电话接入的本地电话公司获得DSL因特网接入（digital subscriber line, DSL）。 所以，本地电话公司就是ISP, 每个用户的DSL调制解调器使用现有的电话线（双绞铜线）    和      位于本地的电话公司本地中心局（CO）中的数字用户线    接入复用器（DSLAM）    来交换数据     =========>      家庭的DSL 调制解调器得到数字数据后将其转换成高频音， 以通过电话线传输给本地中心局，  来自许多家庭的模拟信号在DSLAM处被转换回数字形式

10. 家庭电话线承载了数据和传统的电话信号， （频分复用技术）1. 高速下行信道， 位于50kMz-1MHz  2. 中速上行信道， 位于4kHz 到 50kHz  3. 普通的双向电话信道， 位于0到4kHz频段。 **这种方法把单根DSL线路看起来就像3根单独的线路，因此一个电话呼叫和一个因特网连接能同时共享DSL链路， 在用户一侧，一个分频器把到达家庭的数据信号和电话信号分开，并把数据信号转发给DSL调制解调器。          在本地中心局中， DSLAM把数据和电话信号分开，并把数据送往因特网中**


11. 另一种方式（电缆因特网）vs DSL。利用了有线电视现有的电视基础设施。  

### Online data has to know where it is going
TCP/IP  
Transmission control protocol and internet protocol


#### Application layer
* Browser http/smtp/ssh/ftp
#### Transport layer
* TCP/UDP
#### Internet layer 
* IP

*--- The above three are the ones that we as a developer are most interested in*  
*--- socketio is going to use http and tcp a lot*  
*--- http use tcp as the transport layer*
 <br><br>

#### network layer
* MAC
#### Link (wifi, ethernet) and physical(cables) => network layer

After the application layer gets the data from whatever program you are using, it talks to transport layer through port. Each port can be assigned to a different protocol in the application layer, So TCP knows where the data is coming from . For example most activity in your web browser will go through port 80 which is what HTTP always uses. 

Once TCP gets the data, it chops it up in small chunks called packets so that they can be individually take the quickest route over the internet to get whrerever it is. 
TCP slaps a header to each packet that contains instructions on what order to reasemble the packets as well as error checking information. 

After this, the packets are pushed to internet layer, which uses the internet protocal or IP to attach both the origin and the destination IP address. 

The data is then sent through network layer that handles things like mac addressing so that packets go to the right physical machine


### Example
* You have a computer with internet connection. The transport layer creates 2^16(75000) ports on your computer.

### Socket
* Websocket will allow you to connect to a server's socket via TCP rather than HTTP
* If the server does not allow websocket, socket io will fall over to Long polling, etc
* Every process have tube comeing out of them, out of them can come and go data. 


### Headers
App=> Http, ftp, ssh [(header: metadata, for example, Content-type:text/html) + body]   
Trans => TCP/UDP [(header: source port/Destination port/sequence number/acknowledge number)+http message]
Network => IP [(header: checksum/source ip/destination ip) + tcp message  ]   
Link=> (header:...)  
physical => (header:...)   
