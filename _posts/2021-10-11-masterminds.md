---
layout: post
title: "TryHackMe: Masterminds"
date: 2021-10-11
categories: tryhackme
---

Hello there, this is a write-up for a TryHackMe room that I found quite interesting. The Masterminds room is about 'analyzing malicious traffic using <a url = "https://github.com/brimdata/brim/wiki">Brim</a>', an open source tool, able to analyze packet captures and structured logs. As its github page presents, Brim can be used along with Wireshark, to enhance networking and security investigations of captured traffic. Through this room we will investigate 3 different senarios. I can't hold on any longer, lets just jump in. Start your machines and I suggest you to enlarge the split view to a different tab.

<article>
	<h5><b>[Infection 1]</b></h5>
	<p style="margin-top: 2rem">
		Follow the instructions to load the first pcap. We start by searching the victim's IP address. There are many filters and queries we can apply to find it, but since the captured traffic is short, I just filtered http and grabbed the source IP address of an outbound packet.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim1.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		Moving foward, we are informed that "The victim attempted to make HTTP connections to two suspicious domains with the status '404 Not Found'". We are looking for the domain.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim2.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		The unsuspecting victim continued making dangerous http requests and, now, he/she established a successful HTTP connection. Somehow we know that the body length of the packet is 1,309, lets find it. For the answer we need to provide both the domain and the IP address we found, separated by a comma and no spaces.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim4.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		If you look at the left of the window, there is a tab named Queries. It provides us with a number of usefull queries to make our research easier. You can try some. The one we need to answer is the Unique DNS Queries.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim5.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		We know the domain we are looking for, we just need the uri. In Brim you can filter the domain as 'host'. Don't forget the ==.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim6.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		I couldn't think of any better filter, there must be, but since we are looking for a downloaded executable, we can search for anything that matches with the '.exe'. I will rethink that and probably update with a better solution.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim7.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		With the intel we gathered, we are able to search VirusTotal for the malware that infected this host. Remember, you know from which domain the malware was downloaded to the host. Check the hint.
	</p>
	<h5><b>[Infection 2]</b></h5>
	<p style="margin-top: 2rem">
		The second infected machine waits for you. Keep going. Find the IP address of the victim's machine and jump to the next question.We now need to make a query to filter out only the POST connections. We can use the same query to include the count of them and answer the third question as well.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim8.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		Again looking for a downloaded file, a binary to be specific. It can be a bin, an exe etc., here is again an exe. Same (but not good enough) query as with the previous infection. The uri and the IP address are also visible, so answer the next two questions along with this.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim9.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		You may think, OK, we did whatever we would do while analyzing a pcap with Wireshark right? Well, look at the next question. With Brim we can filter Suricata alerts. That's the security investigation enhencement we've been looking for! You can use the following query to see every packet that was marked with a Suricata alert.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim11.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		Closing with this task, you can now search for trojan that infected the machine.
	</p>
	<h5><b>[Infection 3]</b></h5>
	<p style="margin-top: 2rem">
		One more machine got infected. This will be a long day xD. You know what to do for the first question.<br>
		Moving on, we are looking for 3 (yes!) C2 domains (btw, the Wikipedia entry for a C2 domain is not cybersecurity relative, search for Command and Control 8-)). To answer this question I had to form a query. But, I wasn't aware of Brims query language and it took me some time to get what I wanted. I suggest you two articles that helped me, <a href='https://medium.com/brim-securitys-knowledge-funnel/five-elegant-brim-queries-to-threat-hunt-in-zeek-logs-and-packet-captures-30eec4c09933'> the first</a> and <a href='https://medium.com/brim-securitys-knowledge-funnel/investigating-network-traffic-activity-using-brim-and-zeek-97efdf725f8e'>the second</a>. So, again filtering out for an exe, I got these entries. The IP addresses are also visible.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim12.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		Unique DNS queries once more. This time we need only those that were made to the first IP address we found for the previous question.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim13.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		As I said, the pcaps are short, so we don't have to expand the queries that much. The following query did the trick but, for a longer pcap, we should add the filter <i>uniq</i> and get the count of the returned results. Good thing is that, the user-agent is visible, so we can answer two questions at the time
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim14.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		You see, the answer for this question is 3digit. Don't try to count them, use count instead.
		<img src="/securityegg/assets/images/tryhackme/masterminds/brim15.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
		Finally, some OSINT and you can find the malware, a worm this time, that infected the third machine. <br>
	</p>
	Hope you had fun with this room, I really enjoyed getting hands-on with Brim for the first time.
</article>
