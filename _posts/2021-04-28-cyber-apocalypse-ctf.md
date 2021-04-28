---
layout: post
title: "HTBxCryptoHack Cyber Apocalypse 2021"
date: 2021-04-28
categories: ctfs
---

Five days and 62 challenges after, one can say that this ctf was a highlight. That was just the second ctf I participated and I joined team <a href="http://www.pdsn.uniwa.gr/profile/inssec/" target="_blank">
INSSEC</a> of the University of West Attica. Thankfully, INSSEC has a lot of good players, so we managed to reach the 102th place out of 4740 teams. I managed to contribute with only 4 challenges, 
2 cryptos ( PhaseStream 1 & 2), 1 misc (Input as a Service) and 1 forensics (Alienphish).

<h2 style="margin-top:5%">PhaseStream 1</h2>
<h5> Category: Crypto </h5>
<article style="padding: 1%; border-left: 2px solid">
	<i>"The aliens are trying to build a secure cipher to encrypt all our games called "PhaseStream". 
	They've heard that stream ciphers are pretty good. The aliens have learned of the XOR operation which is used to encrypt a plaintext with a key. 
	They believe that XOR using a reapeted 5-byte key is enough to build a strong stream cipher. Such silly aliens! 
	Here's a flag they encrypted this way earlier. Can you decrypt it (hint: what's the flag format?) 2e313f2702184c5a0b1e321205550e03261b094d5c171f56011904"</i><br>
	<p style="margin-top: 2rem">Not the proudest solution out there, but still one fast enough for a ctf. We know that the flag is encoded via XOR function and that the first 5 characters have
	to be "CHTB{". I just copied the encoded flag and pasted to <a href="https://www.dcode.fr/xor-cipher" target="_blank">dcode.fr</a>. I then decoded with "CHTB{" as key to get the
	real encryption key. Less is more, I guess...
	</p>
</article>

<h2 style="margin-top:5%">PhaseStream 2</h2>
<h5> Category: Crypto </h5>
<article style="padding: 1%; border-left: 2px solid">
	<i>The aliens have learned of a new concept called "security by obscurity". Fortunately for us they think it is a great idea and not a description of a common mistake. 
	We've intercepted some alien comms and think they are XORing flags with a single-byte key and hiding the result inside 9999 lines of random data, Can you find the flag?</i><br>
	<p style="margin-top: 2rem"> For this challenge, we've been provided with an output.txt of 10000 lines. But, lets leave this file for now and find the hints in the description.
	It mentions that, the aliens are now using "a single-byte key" to encrypt their flag. They use the <span style="color:green">single-byte xor cipher</span>!<br>
	This time, a little more effort is needed in order to solve this challenge.
	</p>
	<p style="margin: 2%">
	<code style="color: blue">
	import binascii<br><br>

		f = open('output.txt', 'r')<br>
		lines = f.readlines()<br><br>

		for line in Lines:<br>
			&emsp;&emsp;for i in range(0x00,0xff):<br>
				&emsp;&emsp;&emsp;result = ''<br>
				&emsp;&emsp;&emsp;cipher = binascii.unhexlify(line.strip())<br>
				&emsp;&emsp;&emsp;&emsp;&emsp;for j in cipher:<br>
					&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;result += chr(i^j)<br>
					&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if 'CHTB{' in result:<br>
						&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;print('flag:', result)<br>
		f.close()
	</code>
	</p>
	<p>
	We now only have to run the code and wait for the result.<br>
	<img src="/securityegg/assets/images/ctfs/cyberapocalypse/stream2.png" alt="#" style="margin-top: 2%; max-width: 100%">
	</p>
</article>

<h2 style="margin-top:5%">Input as a Service</h2>
<h5> Category: Misc </h5>
<article style="padding: 1%; border-left: 2px solid">
	<i>In order to blend with the extraterrestrials, we need to talk and sound like them. Try some phrases in order to check if you can make them believe you are one of them.</i><br>
	<p style="margin-top: 2rem">
	This time we get to talk with the not-so-friendly aliens. But we need to "sound like them" so... lets try to say something and see how they will respond. We have an IP and a port 
	to communicate with our extraterestial intruders.<br>
	If we try to greet them, like saying "Hey!", we get the following error: <br>
	</p>
	<p style="margin:2%">
	<code style="color: blue">
	<i>File "/app/input_as_a_service.py", line 12, in main text = input(' ')</i>
	</code>
	</p>
	<p>
	We found the vulnerability. Is the <a href="https://www.geeksforgeeks.org/vulnerability-input-function-python-2-x/" target="_blank">python's input</a> function. It's a vulnerability
	that exists only in the 2.x versions of python and can be exploited by providing another function's as input. In this case, the program will normally execute the input
	we passed. Just give it a try to see if that's the case.
	<img src="/securityegg/assets/images/ctfs/cyberapocalypse/input.png" alt="#" style="margin-top: 2%; max-width: 100%">
	</p>
</article>

<h2 style="margin-top:5%">Alienphish</h2>
<h5> Category: Forensics </h5>
<article style="padding: 1%; border-left: 2px solid">
	<i>Given the Alien Weaknesses.pptx file, get the flag</i>
	<p style="margin-top: 2rem">
	Since this is a forensics challenge I will break my progress down to steps, as I find this method a lot more convenient and suitable for this category.
	<ul style="list-style-type:circle">
	<li style="margin: 2%">
		We have a <span style="color: darkblue">.pptx</span> file with it's content. If we unzip it, we will find a directory tree consisting of numerous files in
		different directories.
	</li>
	<li style="margin: 2%">
		The one with the most interest is the ppt directory. There are many ways to search it but, with some luck, I found what I was looking for pretty easy. What I 
		mean is, we have a powerpoint slideshow so, the items of interest are it's slides. If we navigate to the slides folder we'll find the <span style="color:darkblue">
		slide1.xml</span> and the <span style="color:darkblue">_rels</span> dir. Change to the _rels and you will encounter your target.
	</li>
	<li style="margin: 2%">
		I opened the <span style="color:darkblue">slide1.xml.rels</span> file with Firefox, because it looks better that way (xD).	
	</li>
	<img src="/securityegg/assets/images/ctfs/cyberapocalypse/encoded_exe.png" alt="#" style="max-width: 100%">
	<li style="margin: 2%">
		Take a good look at the highlighted text. There's clearly a cmd.exe call that could be malicious. But, what else is being called? We can recognize two reverse
		part of the string: <span style="color:red">ptth</span> and <span style="color:red">eliftuo</span>. Hmm...
	</li>
	<li style="margin: 2%">
		Why don't we reverse the most promising part with python?
	</li>
	<img src="/securityegg/assets/images/ctfs/cyberapocalypse/encoded_reversed.png" alt="#" style="max-width: 100%">
	<li style="margin: 2%">
		And now to the <a href="https://gchq.github.io/CyberChef/" target="_blank">CyberChef</a>
	</li>
	<img src="/securityegg/assets/images/ctfs/cyberapocalypse/magic.png" alt="#" style="max-width: 100%">
	</ul>
	</p>
</article>
