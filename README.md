Download Link: https://assignmentchef.com/product/solved-solvedprogram-5
<br>
Program DescriptionYou will develop a simple Internet Ping Server and a corresponding Client. The functionality provided by the Client and Server are similar to the standard ping programs available in modern operating systems. However, your Client and Server will be using UDP (instead of ICMP) to communicate with each other. The Server sits in an infinite loop listening for incoming UDP packets. When a packet comes in, the Server simply retrieves the payload, encapsulates the payload in an UDP packet, and sends the packet to the Client. In other words, the Server is echoing the Client’s message. In this case, your Client is a Pinger. It must send 10 ping messages (UDP packets) continuously to the Server and record the RTT (round-trip time) when the Server echoes back the UDP packet. You MUST use an UDP packet size of 512 bytes. The Client must output the minimum RTT, maximum RTT, and the average RTT at the end of program execution.

The Ping ServerYou must have a PingServer class with UDP service. You can use any port number between 5700 and 5800 to receive the UDP packets. The Server MUST inject artificial loss to simulate the effects of network packet loss. That is, you must define a constant LOSS_RATE = 0.3 that determines the percentage of packets should be lost, and a constant AVERAGE_DELAY = 100 (milliseconds) to simulate transmission delay. Set AVERAGE_DELAY = 0 to find out the true RTT of your packets. Here are some useful methods: Random random = new Random(new Date().getTime());

//generate a random number between 0 and 1; it’s a packet loss if the random number is less than LOSS_RATE DatagramSocket udpSocket = new DatagramSocket(PORT_NUMBER); //create the socket for receiving UDP packets byte[] buff = new byte[PACKET_SIZE]; //allocate the memory space for an UDP packet

DatagramPacket inpacket = new DatagramPacket(buff, PACKET_SIZE); //make an empty UDP packet udpSocket.receive(inpacket); //receive the next UDP packet

Thread.sleep((int)(random.nextDouble() * DOUBLE * AVERAGE_DELAY)); //simulate transmission delay; DOUBLE =

2 DatagramPacket outpacket =

new DatagramPacket(payload, payload.length, clientAddr, clientPort); //make an outgoing UDP packet udpSocket.send(outpacket); //send an UDP packet

The ClientYour Client should send 10 ping messages to the Server, with one second (1000 milliseconds) interval. Each message contains a payload of data that includes the keyword PING, a sequence number, and a timestamp. After sending each packet, the client waits up to one second to receive a reply. If one second goes by without a reply from the server, then the client assumes that its packet or the server’s reply packet has been lost in the network. And the Client prints an error message identifying packet loss to the system output. You will need setSoTimeout() method in DatagramSocket class to set the timeout value on a datagram socket. Set 5 second timeout (5000 milliseconds) for collecting pings after 10 pings were sent, just to wait for possible delayed replies from the Server. You must include a PingClient class, a PingMessage class, and an UDPPinger class. Follow the instructions below.

PingMessage classUse this class to create a ping message to be sent to the Server. The format of a ping message is shown below. You must provide the following public methods.

public PingMessage(InetAddress addr, int port, String payload)

//constructor public InetAddress getIP() //get the destination IP address public int getPort() //get the destination port number public String getPayload() //get the content of the payload

Destination IP addressDestination Port NumberPayload

The payload part of each PING message is a String with the following format. You can use the getTime() method in Date class to get the current time as the timestamp.

PINGSPACESequence NumberSPACEtimestamp

UDPPinger classUse an instance of this class to send a ping message and receive the echo message from the Server. You must provide 2 public methods. You may need .getAddress(), .getData() and .getPort() in DatagramPacket class.

/**

This method sends an UDP packet with an instance of PingMessage.

Use the constructor DatagramPacket(byte[] payload, int length, InetAddress address, int port) to construct an UDP packet and send the UDP packet.

*/

public void sendPing(PingMessage ping)

//This method receives the UDP packet from the Server. This method may throw SocketTimeoutException. public PingMessage receivePing()

PingClient classThis is the Client class. DO NOT use JFrame. This class extends UDPPinger class, and implements the Java Runnable interface. That is, you MUST implement the run() method. In the run() method, you MUST

(1) Create an instance of DatagramSocket.

(2) Set the time out for the socket to be 1 second (1000 milliseconds)

(3) Set up a for loop to send 10 PING messages, record the replies from the Server, and compute the RTT.

(4) Wait additional 5 seconds (set time out to 5 seconds) after 10 PINGs are sent (if less than 10 replies are received), just in case replies are on the way.

(5) Compute and output the average RTT, minimum and maximum RTTs. Use the following formula. Set the RTT to 1000 milliseconds in case of a packet loss.

RTT = (current timestamp) – (previous timestamp when sending the packet)

Program Requirement1. Your programs MUST NOT crash under any situation.

2. You MUST try-catch everything and display appropriate error messages identifying the specific exception.

3. You MUST display information about ping messages on Server’s system output, and on Client’s system output. Server’s sample output

Ping Server running….

Waiting for UDP packet….

Received from: /127.0.0.1 PING 0 1446311917208Reply sent.

Waiting for UDP packet….

Received from: /127.0.0.1 PING 1 1446311917351Reply sent.

Waiting for UDP packet….

Received from: /127.0.0.1 PING 2 1446311917475 Reply sent.

Waiting for UDP packet….Received from: /127.0.0.1 PING 3

1446311917565 Packet loss…., reply not sent.

Waiting for UDP packet….

Client’s sample outputContacting host: 127.0.0.1 at port 5701

Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:37 CDT 2015

Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:37 CDT 2015 Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:37 CDT 2015 receivePing…java.net.SocketTimeoutException: Receive timed out receivePing…java.net.SocketTimeoutException: Receive timed out receivePing…java.net.SocketTimeoutException: Receive timed out Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:40 CDT 2015 Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:40 CDT 2015 receivePing…java.net.SocketTimeoutException: Receive timed out Received packet from /127.0.0.1 5701 Sat Oct 31 12:18:41 CDT 2015

PING 0: true RTT: 143 PING 1: true RTT: 124

PING 2: true RTT: 90

PING 3: false RTT: 1000

PING 4: false RTT: 1000

PING 5: false RTT: 1000

PING 6: true RTT: 52

PING 7: true RTT: 105

PING 8: false RTT: 1000

PING 9: true RTT: 99

Minimum = 52ms, Maximum = 1000ms, Average = 461.3ms.

4. You MUST follow the software development ground rules from CS2430 (posted on D2L.)

5. You MUST demo your program by the due date, or 0 points.

Program GradingExceptions/ViolationsEach OffenseMax OffProgram not running2020Failed to ping localhost2020Failed to ping a remote Server1010**Either of the group members failed to show up for the demo (if work as a group of 2)2020**Missing time logs (if work as a group of 2)55For each method/class required but not implemented39Incorrect minimum, maximum, average RTT39Incorrect ping message format33Improper handling try/catch exceptions24Improper or insufficient messages on system output15