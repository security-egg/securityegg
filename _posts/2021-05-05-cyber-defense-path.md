---
layout: post
title: "TryHackMe: Cyber Defense Learning Path P1D3"
date: 2021-05-05
categories: tryhackme
---

We could say that, network traffic and Wireshark are getting along as much as crêpes with chocolate and strawberries ( I will accept no argument on that). Often in forensic challenges you 
will have to analyze a .pcap file. Wireshark is the tool you need. It will present you a nice overview of packets transmitted, categorized by protocol, time, source and and destination 
address and that's not all. Wireshark provides, as well, a number of filters you may use in order to preview the results of a capture in your own, customized way. Are you ready to dive 
deeper?<br>
Today, I'm gonna skip to Task 7. The previous tasks have a bunch of usefull informations about the tool, giving us the heads-up we need. For the rest tasks, we will have to download the 
pcap files and examine them further.
<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h3>Wireshark 101</h3>
<p style="margin-top: 2rem">
<h5><b>Task 7</b></h5>
Our first stop is on ARP traffic. Address Resolution Protocol is the protocol that acts as a bridge between MAC and IP addresses.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the Opcode for Packet 6?</b></i><br>
	After clicking on packet 6 you have to check the Address Resolution Protocol (request) tab from it's description. The Opcode is <u>request (1)</u>
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the source MAC Address of Packet 19?</b></i><br>
	You need the MAC address, not the IP. Once more, click on packet 19. The answer is 80:fb:06:f0:45:d7
    </li>
    <li style="margin-top: 1rem">
     <i><b>What 4 packets are Reply packets?</b></i><br>
	Have you thought of applying a filter? Try arp.opcode == 2. The packes are <u>76,400,459,520</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What IP Address is at 80:fb:06:f0:45:d7?</b></i><br>
	The same MAC from the second question. Go back to packet 19. 10.251.23.1 is your address.
    </li>
  </ul>
<h5><b>Task 8</b></h5>
Moving on with OSI's layers, ICMP is the protocol that works with the famous ping tool (remember from the Introductory Networking room). Tip time: I recently learned from a ctf challenge 
that, among all the exfiltration attack types there is also the ICMP exfiltration attack. Keep that in mind and learn how to examine ICMP traffic.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the type for packet 4?</b></i><br>
	It's an Echo (ping) request of type <u>8</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the type for packet 5?</b></i><br>
	You can also google of the different ping types. A reply ping is of type <u>0</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the timestamp for packet 12, only including month day and year?</b></i><br>
	Click on the packet. The answer is May 30, 2013.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the full data string for packet 18?</b></i><br>
	Just copy and paste 08090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f202122232425262728292a2b2c2d2e2f3031323334353637
    </li>
  </ul>
<h5><b>Task 10</b></h5>
The already called (by me) Savior, DNS can make it's own traffic trail. I referenced before the ICMP exfiltration attack but, a way more "popular" one is the DNS exfiltration attack.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is being queried in packet 1?</b></i><br>
	You have to check the field Queries after the Domain Name System tab. The answer is 8.8.8.8.in-addr.arpa.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What site is being queried in packet 26?</b></i><br>
	The last column, named Info, is giving us the answer. <u>www.wireshark.org</u>
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the Transaction ID for packet 26?</b></i><br>
	Look at the Domain Name System tab. The first thing you'll see is the Transaction ID: <u>0x2c58</u>.
    </li>
  </ul>
<h5><b>Task 11</b></h5>
A familiar protocol, isn't it? Hypertext Transfer Protocol is the layer's 7 protocol that transforms data to a human readable form.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What percent of packets originate from Domain Name System?</b></i><br>
	Look at the top of the wireshark's window and select Statistics > Protocol Hierarchy. That's a good start for analysing pcaps from forensics challenges. For this question, 
	the answer is 4.7.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What endpoint ends in .237?</b></i><br>
	Once more, go to Statistics and then Endpoints. Now think. A .237 looks a lot like the last 8 bits of an IPv4 address. The full is 145.254.160.237.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Looking at the data stream what is the full request URI from packet 18?</b></i><br>
	Pretty long so copy it, it's https://pagead2.googlesyndication.com/pagead/ads?client=ca-pub-2309191948673629&random=1084443430285&lmt=1082467020&format=468x60_as&output=html&url=http%3A%2F%2Fwww.ethereal.com%2Fdownload.html&color_bg=FFFFFF&color_text=333333&color_link=000000&color_url=666633&color_border=666633
    </li>
    <li style="margin-top: 1rem">
     <i><b>What domain name was requested from packet 38?</b></i><br>
	You need just the domain, meaning <u>www.ethereal.com</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Looking at the data stream what is the full request URI from packet 38?</b></i><br>
	And now the full request, <u>http://www.ethereal.com/download.html</u>.
    </li>
  </ul>
<h5><b>Task 12</b></h5>
Same with HTTP but Secure, HTTPS keeps protects your data by encrypting them. Note: If you can't find the SSL protocol you have to select the TLS > RSA keys list > Edit. Google why.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Looking at the data stream what is the full request URI for packet 31?</b></i><br>
	With the data now decrypted we can get that, the full request is https://localhost/icons/apache_pb.png.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Looking at the data stream what is the full request URI for packet 50?</b></i><br>
	https://localhost/icons/back.gif
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the User-Agent listed in packet 50?</b></i><br>
	Mozilla/5.0 (X11; U; Linux i686; fr; rv:1.8.0.2) Gecko/20060308 Firefox/1.5.0.2\r\n
    </li>
  </ul>
</p>
</article>