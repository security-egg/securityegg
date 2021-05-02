---
layout: post
title: "TryHackMe: Cyber Defense Learning Path P1D1"
date: 2021-05-01
categories: challenges
---

One of the reasons I like TryHackMe so much, and maybe the main reason, is the Learning Paths it offers. Right now, there are 5 learning paths with different number
of rooms, each one. Cyber Defense is the one with the most rooms, 38, and it claims that 48 hours are needed to be completed. A lot of knowledge waits in there to be
acquired so... lets start with the first part.

<h2 style="margin-top:5%">Cyber Defense Introduction</h2>
<p>The first part includes basic stuff about networking and the Windows OS, splitted into 6 rooms.
According to it's description, we will <i>"Learn the basics of networking, host-based systems, and active directory. 
These rooms will give you the foundational knowledge needed to grasp more advanced concepts".</i> Now, for a student of informatics,
 especially for one that follows a networking academic flow, the networking rooms can be fullfiled in minutes. But, for someone that gets she's/he's
first impression of networking, it's a really good introduction.
</p>
<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h3>Introductory Networking</h3>
<p style="margin-top: 2rem">
<h5><b>Task 2</b></h5> This task is about the OSI model, the most basic way that data gets partitioned and travels in telecommunications systems.
After reading the information for each layer of the model, we will be able to answer the following questions.
  <ul style="margin-top: 2rem">
    <li>
     <i><b>Which layer would choose to send data over TCP or UDP?</b></i><br>
	TCP stands for Transmission Control Protocol and UDP for User Datagram Protocol. Both are protocols of the Transport layer, so the correct answer is 4.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer checks received packets to make sure that they haven't been corrupted?</b></i><br>
	Data Link layer is the last layer before data gets transmitted through a cable and the first after received. Isn't this the best time to check if something went wrong while data was 
	travelling across the network? That's why data link <i>"serves an important function when it receives data, 
	as it checks the received information to make sure that it hasn't been corrupted during transmission"</i>. Correct answer: 2.
    </li>
    <li style="margin-top: 1rem">
     <i><b>In which layer would data be formatted in preparation for transmission?</b></i><br>
	As we said before, data link is the step before data pass in a cable. The answer, again, is 2.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer transmits and receives data?</b></i><br>
	Yes, you know the right answer. It's the physical layer, 1.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer encrypts, compresses, or otherwise transforms the initial data to give it a standardised format?</b></i><br>
	We have encryption, compression or any other way to <u>represent</u> the data? Of course, the Presentation layer, meaning 6.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer tracks communications between the host and receiving computers?</b></i><br>
	If I wanted to communicate with another computer, in the OSI language I would try to <u>establish a session</u>. You got it, the right answer is 5,
	the Session layer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer accepts communication requests from applications?</b></i><br>
	A layer that speaks with applications? It isn't that hard, right? Layer 7, the higher one in the OSI model, is the Application layer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer handles logical addressing?</b></i><br>
	Read the question carefully. We want the layer that understands <u>logical</u> addressing, not physical. A logical address, in the networking world, is an IP address.
	That can only means one thing. Networking layer, number 3.
    </li>
    <li style="margin-top: 1rem">
     <i><b>When sending data over TCP, what would you call the "bite-sized" pieces of data? </b></i><br>
	Well, I can't find a good mnemonic way to remember how data are called in each layer, I think is mostly about experience. Any way, scrolling a bit up and we will find
	the right answer. For the TCP data are called "Segments".
    </li>
    <li style="margin-top: 1rem">
     <i><b>[Research] Which layer would the FTP protocol communicate with?</b></i><br>
	Some googling and we have the right answer, is the Application layer, 7.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which transport layer protocol would be best suited to transmit a live video?</b></i><br>
	I remember a nice meme about Skype and how most of the time of a video call is like "Can you hear me?". A protocol that can ensure data loss but, it's ok, we care mostly
	for the speed. It's the UDP.
    </li>
  </ul>
<h5><b>Task 3</b></h5><br>
In this task we observe the encapsulation progress of data in the OSI model. What happens here is that, from top to bottom, each layer adds a matching header containing information 
and enhancing security. The complete packet will be transmmited and, when received, de-encapsulated to what the actual payload is.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>How would you refer to data at layer 2 of the encapsulation process (with the OSI model)?</b></i><br>
	The only way to solve this question, as also the rest that follows, is with the help of the image that the room provides. In this case, the right answer is "Frames".
    </li>
    <li style="margin-top: 1rem">
     <i><b>How would you refer to data at layer 4 of the encapsulation process (with the OSI model), if the UDP protocol has been selected?</b></i><br>
	"Datagrams"
    </li>
    <li style="margin-top: 1rem">
     <i><b>What process would a computer perform on a received message?</b></i><br>
	The reverse process of encapsulation is, you wouldn't imagine xD, de-encapsulation.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which is the only layer of the OSI model to add a trailer during encapsulation?</b></i><br>
	While answering some questions in the previous task, I pointed out the importance of the <u>Data Link layer</u>. Is the last one before data transmission through the Physical layer, 
	thus a lot of important functions are taking place there. Is the layer that delivers the complete data stream to a cable, so is also the one that "closes" the encapsulation progress 
	by adding a trail, as long as with a header.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Does encapsulation provide an extra layer of security (Aye/Nay)?</b></i><br>
	Aye, aye captain!
    </li>
  </ul>
<h5><b>Task 4</b></h5><br>
Networking family time! We get to meet with OSI's predecessor, the TCP/IP model. They maybe look pretty similar, but notice the differences at their highest and lower levels.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Which model was introduced first, OSI or TCP/IP?</b></i><br>
	A lot simpler than the 0SI model, TCP/IP was introduced some years before.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer of the TCP/IP model covers the functionality of the Transport layer of the OSI model (Full Name)?</b></i><br>
	<u>Transport</u> is transport, no matter of the model.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer of the TCP/IP model covers the functionality of the Session layer of the OSI model (Full Name)?</b></i><br>
	OSI's Session, Presentation and Application layers are all included into TCP/IP's <u>Session</u> layer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>The Network Interface layer of the TCP/IP model covers the functionality of two layers in the OSI model. These layers are Data Link, and?.. (Full Name)?</b></i><br>
	Try not to get confused here. TCP/IP's Network Interface is <u>NOT</u> the OSI's Network layer. In contrast, it includes the Data Link and <u>Physical</u> layers.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which layer of the TCP/IP model handles the functionality of the OSI network layer?</b></i><br>
	Keep staying focused, in TPC/IP model the network layers is called Internet.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What kind of protocol is TCP?</b></i><br>
	I hope you didn't miss the picture that describes the TCP 3-Way Handshake Process. One of my favorite things about TPC, it's Connection-based. Do you remember the Skype meme?
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is SYN short for?</b></i><br>
	My beloved handshake again, we said 3-way right? Starting from the SYN, which stands for "Synchronise".
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the second step of the three way handshake?</b></i><br>
	"Did you just sent me a SYN? Do you ACKnowledge that?" The answer is SYN/ACK.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the short name for the "Acknowledgement" segment in the three-way handshake?</b></i><br>
	"Oh, yes sir. I <u>ACK</u>nowledge".
    </li>
  </ul>
<h5><b>Task 5</b></h5><br>
We are about to get our hands dirty with one of the most, or probably THE most, used tools of the networking world. Ping!
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What command would you use to ping the bbc.co.uk website?</b></i><br>
	Action starts here. Try to <u>ping bbc.co.uk</u>
    </li>
    <li style="margin-top: 1rem">
     <i><b>Ping muirlandoracle.co.uk. What is the IPv4 address?</b></i><br>
	You have to ping it if you want to get the answer. The address is 217.160.0.152.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What switch lets you change the interval of sent ping requests?</b></i><br>
	Try ping -h or man ping. You will find the answer, as long as what the "interval of sent ping requests" is. You are looking for -i.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What switch would allow you to restrict requests to IPv4?</b></i><br>
	You're looking only for IPV4 addresses? Try -4.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What switch would give you a more verbose output?</b></i><br>
	I believe, almost every terminal tool has this "verbose" option. And, it's always -v.
    </li>
  </ul>
<h5><b>Task 6</b></h5><br>
Another very usefull and popular networking tool. With traceroute one can observe the "path" a request follows.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What switch would you use to specify an interface when using Traceroute?</b></i><br>
	Traceroute has a man and help page, as well. There you will find that, if you need to specify an interface, you need the -i switch.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What switch would you use if you wanted to use TCP SYN requests when tracing the route?</b></i><br>
	We want to trace the first step of the 3-way TCP handshake. We can do that with the -T switch.
    </li>
    <li style="margin-top: 1rem">
     <i><b>[Lateral Thinking] Which layer of the TCP/IP model will traceroute run on by default (Windows)?</b></i><br>
	Focus! TCP/IP model <u>AND</u> logical addresses. You are in the Internet layer.
    </li>
  </ul>
<h5><b>Task 7</b></h5><br>
Can you ask "Who is Facebook?". Well, in a terminal strange things can happen!
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the registrant postal code for facebook.com?</b></i><br>
	I told you, you just have to ask "whois facebook.com". Whois is such a gossipy tool. The answer is 94025.
    </li>
    <li style="margin-top: 1rem">
     <i><b>When was the facebook.com domain first registered?</b></i><br>
	For people feeling old, Facebook first registered at 29/03/1997.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which city is the registrant based in?</b></i><br>
	I know where you live, Microsoft. Redmond is the answer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>[OSINT] What is the name of the golf course that is near the registrant address for microsoft.com?</b></i><br>
	OSINT means google time. Try google maps. There is a fine golf place located near Microsoft's building, it's called Bellevue Golf Course.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the registered Tech Email for microsoft.com?</b></i><br>
	Scroll a bit and you will find msnhst@microsoft.com.
    </li>
  </ul>
<h5><b>Task 8</b></h5><br>
One more interesting tool. We have some reading to do here but, believe me, <u>information gathering</u> is one of the most valuable skills in this field.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is DNS short for?</b></i><br>
	I would say that the S in DNS stands for savior, as it saves us from a lot of trouble. But that's not the case, as DNS stands for Domain Name System.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the first type of DNS server your computer would query when you search for a domain?</b></i><br>
	If the domain is not already stored in your computer's local cache, the next step will be to request it from a <u>recursive</u> DNS server.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What type of DNS server contains records specific to domain extensions (i.e. .com, .co.uk*, etc)*? Use the long version of the name.</b></i><br>
	Besides .com and .org there are a lot of other extenstions that could conclude a domain. Thus, every <u>Top-Level Domain</u> server handles only specific extensions.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Where is the very first place your computer would look to find the IP address of a domain?</b></i><br>
	You computer's <u>Local Cache</u> may contains a lot of interesting information.
    </li>
    <li style="margin-top: 1rem">
     <i><b>[Research] Google runs two public DNS servers. One of them can be queried with the IP 8.8.8.8, what is the IP address of the other one?</b></i><br>
	Seek and... answer. Besides the most popular 8.8.8.8 DNS server, there's also the <u>8.8.4.4</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>If a DNS query has a TTL of 24 hours, what number would the dig query show?</b></i><br>
	The networking time passess fast. Time To Live is measured in seconds, so you just have to compute 3600 x 24 = 86400.
    </li>
  </ul>
</p>
</article>
<p style="margin-top: 2rem">
Congrats, we just finished the first room! A lot of reading and some practice to build a strong knowledge base on networking.
</p>
