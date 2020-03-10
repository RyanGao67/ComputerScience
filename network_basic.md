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
