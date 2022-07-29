---
layout: post
title: "Hacky Holidays - Unlock the City 2022"
date: 2022-07-28
categories: ctfs
---

Long time no see and I return with a CTF write-up. The one I'm talking about is the Hacky Holidays - Unlock the City, hosted by hackazon, which lasted from 08/07/2022 to 26/07/2022. It released challenges in 4 phases, many many challenges, but, I didnt have the inspiration to get bothered with a CTF before, I believe, the 21st of July.<br>So, I just solved some beginner and easy level challenges, still, I got a taste of many more, as long as failed to see a clear path to the solution of most of them. Which means that, one more post about this CTF will follow, this time with the mistakes I did and led me away from my dearest flags xD.

<article>
	<h3><b>Mayor's Blog | Web | Beginner</b></h3><br>
	This challenge consisted of 5 parts. The first part was about finding a secret flag, somewhere hidden in the website. I am proud to confess that... I couldn't find it. I don't know why, I don't know where they hid it, I just couldn't find it and skipped right to the next part.<br><br>
	<h5>Task 2: Get Access to an Editor Account</h5><br>
	<i>Try to exploit the password resetting functionality to gain access to an account</i><br>
	Indeed, there is a "Forgot Password" functionality. Which I forgot to get a screenshot for. But let's continue, just have faith to my description. The website was hosting the Mayor's Blog. There were two posts from the mayor, a paragraph laughing at the mayor about the bad security mechanisms and a "Click here to login" button. The button led to a login page with the reset password option.<br>
	What's intresting, in order to reset a password we've been asked to answer to two security questions. This weakness leads to an attack vector including <u>social engineering</u>, which I find a pretty good idea, as it is a very realistic approach.<br>
	To realise how bad idea is to implement security questions functionality like this, check out this <a href='https://cheatsheetseries.owasp.org/cheatsheets/Choosing_and_Using_Security_Questions_Cheat_Sheet.html'>OWASP's cheatsheet</a>, which, even, highlights the fact that <i>"Security questions are no longer recognized as an acceptable authentication factor per NIST SP 800-63"</i> as a warn.<br>
	Back to the challenge, we now need to 1) find a username and 2) answer the two security questions, "Best friend from childhood" and "Date of birth". Luckily, everything we need can be found in the first page of the website, the one I described before and now it's time for you to see aswell (lol).<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/2_info_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	The username we are looking for is "the_mayor", which you can see in the "Posted by" field of the posts. Now, the fella that hacked the site before as made a statement, which helps as a lot as it accuses the mayor of discriminations in favor of his <i>best friend</i>, George. I don't know that happens in this town, but I know that we found the answer to one of the security questions.<br>
	Looking a bit lower, you can see a post where the_mayor thanks the people that congratulated for his <i>50th</i> birthday. It was posted on <i>24-jan-2022</i>. And now the part I didn't pay notice to, at first, and I was wondering what is going on. The word yesterday (attention: you can place your facepalm here).<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/2_reset.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	OK, we have everything we need, we hit the reset button and hope it works.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/2_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	This is the flag. What a satisfaction.<br><br>
	<h5>Task 3: Escalate to Administrative Privileges</h5>
	<i>It looks like the design of the authentication system is very flawed. Find out how users are authenticated and exploit the system to gain Administrator rights.</i><br>
	Check what the task asks one more time. <u>Find out how users are authenticated</u>. It is time to intercept the login request with Burp.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/3_admin_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Pretty clear, the cookie can be read as plain text and there is a field about the admin privileges of the logged in account. Which can be edited. To Admin. True. Kudos to the Mayor's Blog sysadmin (spoiler alert for the next task??).<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/3_logged_as_admin_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	And this is the story of how I became Administrator of the Mayor's blog. Oh, cool, I can even see some nginx logs from here.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/3_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	And a flag?<br><br>
	<h5>Task 4: Gain Access to the Developer Console</h5>
	<i>There is a special console for administrators on the website, but the link is hidden. Find the link, and gain relevant information from the logs. In the end, use the console to find user information.</i><br>
	So, what happened here is that, the sysadmin thought,<i> "I have a shell with admin privileges stored in here, just in case I will need it??, but I don't want to be crawled, I need to hide it. Let's add it to a robots.txt"</i>.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/4_robots.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	I will be fair with our sysadmin here, even though the admin_shell was not, really, hidden, it couldn't be accessed even by altering the cookie to admin. I had to find a way arround to this. So, lets take a step back and check these logs we discovered before. And I mean, thorough check (I spent some time trying to make my way with an SSRF exploit. Time wasted? never).<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/4_sysadmin.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Is that the sysadmin's account password? In plain text again? Didn't you have one job?<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/4_logged_as_sysadmin.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Climbing to the top of this blog. Do you believe that now I gained access to the admin_shell?<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/4_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	With ls and cat. Making my life simple.<br>
	<h5>Task 5: Crack the Password of the Admin Account on the Server</h5><br><br>
	<i>Find a vulnerability in the developer console and use it to access a password file.</i><br>
	For this task I didn't manage to find the flag. But, here's what I have.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/5_pass_shad.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	I tried some combinations of piping and merging commands with the ';' operator, but it was way easier. With the cat command we could traverse back from the current path to any other. That didn't work with the ls command. I managed to retrieve the shadow and passwd files from the server file system, but I couldn't break them with john. And I tried. So many times. Maybe the answer was something else, or I didn't set john right, anyways, here's the vulnerability, or a vulnerability.
</article><br><br>
<article>
<h3><b>You Can't See Me | Network, ICS | Easy</b></h3><br>
	Moving on, we have a network forensics challenge, with two captures for us.<br><br>
	<h5>Task 1: Parts Make a Whole</h5><br>
	<i>The file shows communication between a PLC and an ICS workstation. Analyze the file to get the flag!{use modbus.pcapng)</i><br>
	I'm always starting a pcap analysis by checking the Protocol Hierarchy tab. You can find it under Statistics. Why? Because, realistically, network captures are long and consist of packets originating from many sources (check the Endpoints tab for that), under different layers and protocols. Most of them are part of a legitimate traffic, so if you just keep scrolling the traffic to find a trace of the attack, you're just looking for a needle in the haystack. We need to clear a path through that and understand what is going on this in network, what is supposed to be common and what behavior seems deviant.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/modbus_protocols.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	So, what to keep from this screenshot. We see traffic only under the TCP, which includes HTTP traffic, with data, and Modbus/TCP traffic. We know that we need to clarify the communication between <u>a PLC and an ICS workstation</u> so, the Modbus traffic is expected. We need to examine these two protocols and, to be honest, Modbus seems way appealing to me. Let's start from there.<br>
	By searching online, I found a nice <a href="https://www.sans.org/posters/modbus-rtu-tcp/">cheatsheet</a>, with very helpful tshark commands, from SANS. Keep that in mind, as it also applies on the Chemical Plant challenge. For this one, as we are looking for a communication between two components, we can filter out the communications.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/1_convs.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	The one with the highest number of frames is the one pointed out. A lot of frames. Notice also that, the one with the IPv4 '192.168.198.138'  communicates with 3 external IPs. We will see about that later on. For now, let's check out what this two component-pals had to say. I returned to the wireshark and filtered out the first IP, as source just to lower the number of packets, and added the data field to my filter.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/1_filter.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	You don't see the whole traffic but, believe me, every packet was of length either 115 or 68. You can also see, in the info, that they are 'acknowledge' packets, of the sequences 1,13. But that's another story, about how the TCP works. Lets just open one of the 115-length packets to see what it's hidding.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/1_sol.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br><br>
	Do you also see a flag in there?<br><br>
	<h5>Task 3: Man in the Middle</h5><br>
	<i>What is the protocol used in the Man-in-the-Middle attack performed by the rogue ICS component in this network?(use network.pcapng)</i><br>
	Don't lose your faith in me, if you have any xD. I solved every part of the challenge, I just prefer to present the 3d part before the 2nd, and I believe you will understand why. I will break the task in parts, just to see how many information we can get, just from the description.<br>
	<u>What is the protocol used in the Man-in-the-Middle attack</u>. We are looking for a protocol, which is also the answer in the format CTF{protocol_in_capital_letters}, which was abused in order to achieve a Man-in-the-Middle attack. That demystifies a lot, as we know the type of the attack and, also, it is one pretty common and documented. For an ICS enviroment communicating with Modbus, I found a <a href="https://dispel.io/blog/mitre-att-ck-for-ics-external-remote-services/">nice article</a> covering the MITTRE ATT&CK vector of the Man-in-the-Middle attack we expect to see.<br>
	<u>performed by the rogue ICS component</u>. Please, hold on that in order to understand why I prefer this task order. We are looking for a rogue component, compromised by a mitm attack. And, from the aforementioned article, we know that this attack is an ARP poisoning.<br>
	How can we find traffic indicating ARP poisoning? Well, the protocol hierarchy is there to help us again.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/network_protocols.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br><br>
	Now we have TCP as long as UDP traffic. We will check about that later because, we now care for the ARP traffic. Filtering that out, it is still too much to process. We need to filter it more. We need to think how the ARP poisoning works. Welp, you can google that and find dozens of sources. I will just get to the point. We are looking for more than one IP addresses matched with the same MAC address. A <a href="https://www.oreilly.com/library/view/packet-analysis-with/9781785887819/ch07s04.html">short post</a> I found for this points out the filter we can use to check for that information. One step away from the solution?<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/2sol_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br><br>
	That's evidence of an ARP spoofing attack we've been looking for.<br><br>
	<h5>Task 2: WHOAMI</h5><br>
	<i>There seems to be some suspicious activity in the network. Can you identify the IP address of the rogue ICS component?(use network.pcapng)</i><br>
	Consider the path of the attack. We saw Modbus traffic from the '.138'. Also, the same IPv4 endpoint was detected spoofing its MAC address. We don't expect our attacker to stay there and not expand to the rest of the network, right?. Why not check the rest of the conversations the '.138' had in the network capture?
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/2_convs.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br><br>
	This is the Conversations tab, again under Statistics. As you can see, the '.138' made the underlined conversations. We know, and examined, from the first task about the communications over Modbus/TCP. That leaves us with the '.128' IP address. Which, who would have though, makes traffic with a number of external IPs.<br> 
	 Which not only seems suspicious, but can be proved extremely harmful.<br>
	<h5>Task 4: Follow Me Till the End</h5><br>
	<i>The rogue component is communicating with an external entity, which is a big red flag in ICS enviroments. Can you find the flag from the network data?(use network.pcapng)</i><br>
	Now I can brag about proving my point xD. Skip that, act like you never saw it.<br>
	What we are looking for now. 1. Communication between the rogue component and an external entity. 2. Network data. Return back to the protocols of this capture. We can clearly see data transfered but, not under the HTTP layer. Just to check what's going on with the http traffic, I filtered it out. Besides the certificate packets, I found one GET request from our rogue component, with source the '192.168.198.138' and destination the '35.232.111.17', but it came with a '204 No Content' response. Nothing much of interest here, apart from the fact that it did tried to receive something from an external source. However, I can see another protocol which is used to establish remote communication and, actually, a remote-control communication.<br>
	Filtering the VNC traffic, there is a packet of 3947 length. I follow the tcp stream and get this.
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/4_packet.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Quite interesting. So now, I wonder, what will happen if i get the data from the img tag out, decode it from base64, as it is suggested from the src, and save it as a png file?<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/cant_see/4_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	As you see, it was easy. And such an enjoyment.
</article><br><br>
<article>
<h3><b>Audible Transmission | Stego | Easy</b></h3><br>
From that challenge, I just got the first, really easy, part. I'm just showing it for the fun, nothing difficult here.<br>
<h5>Task For Your Eyes Only: Follow Me Till the End</h5><br>
<i>Are you able to see what was being sent</i><br><br>
	So, we get a wav file, with a secret message in it. It is stego, so we are taking the usual path, blah blah, I just opened a Sonic Visualizer and added a spectrogram to get this.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/transmission/1sol.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	After that, I completelly skipped (forgot?) the next task of this challenge, as I took my time with two other challenges. Where is that shame gif?<br>
</article>
