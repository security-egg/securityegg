---
layout: post
title: "Hacky Holidays - Unlock the City 2022"
date: 2022-07-28
categories: ctfs
---

Long time no see and I return with a CTF write-up U+1F973. The one I'm talking about is the Hacky Holidays - Unlock the City, hosted by hackazon, which lasted from 08/07/2022 to 26/07/2022. It released challenges in 4 phases, many many challenges, but, I didnt have the inspiration to get bothered with a CTF before, I believe, the 21st of July.<br>So, I just solved some beginner and easy level challenges, still, I got a taste of many more, as long as failed to see a clear path to the solution of most of them. Which means that, one more post about this CTF will follow, this time with the mistakes I did and led me away from my dearest flags xD.

<article>
	<h3><b>Mayor's Blog | Web | Beginner</b></h3>
	This challenge consisted of 5 parts. The first part was about finding a secret flag, somewhere hidden in the website. I am proud to confess that... I couldn't find it. I don't know why, I don't know where they hid it, I just couldn't find it and skipped right to the next part U+1FAE0.
	<h5>Task 2: Get Access to an Editor Account</h5>
	<i>Try to exploit the password resetting functionality to gain access to an account</i><br>
	Indeed, there is a "Forgot Password" functionality. Which I forgot to get a screenshot for. But let's continue, just have faith to my description. The website was hosting the Mayor's Blog. There were two posts from the mayor, a paragraph laughing at the mayor about the bad security mechanisms and a "Click here to login" button. The button led to a login page with the reset password option.<br>
	What's intresting, in order to reset a password we've been asked to answer to two security questions. This weakness leads to an attack vector including <u>social engineering</u>, which I find a pretty good idea, as it is a very realistic approach.<br>
	To realise how bad idea is to implement security questions functionality like this, check out this <a href='https://cheatsheetseries.owasp.org/cheatsheets/Choosing_and_Using_Security_Questions_Cheat_Sheet.html'>OWASP's cheatsheet</a>, which, even, highlights the fact that <i>"Security questions are no longer recognized as an acceptable authentication factor per NIST SP 800-63"</i> as a warn.<br>
	Back to the challenge, we now need to 1) find a username and 2) answer the two security questions, "Best friend from childhood" and "Date of birth". Luckily, everything we need can be found in the first page of the website, the one I described before and now it's time for you to see aswell (lol).<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/2_info_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	The username we are looking for is "the_mayor", which you can see in the "Posted by" field of the posts. Now, the fella that hacked the site before as made a statement, which helps as a lot as it accuses the mayor of discriminations in favor of his <i>best friend</i>, George. I don't know that happens in this town, but I know that we found the answer to one of the security questions.<br>
	Looking a bit lower, you can see a post where the_mayor thanks the people that congratulated for his <i>50th</i> birthday. It was posted on <i>24-jan-2022</i>. And now the part I didn't pay notice to, at first, and I was wondering what is going on. The word yesterday (attention: you can place your facepalm here).<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/2_reset.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	OK, we have everything we need, we hit the reset button and hope it works.<br>
	<img src="/securityegg/assets/images/ctfs/hacky_holidays/mayor's_blog/2_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	This is the flag. What a satisfaction.
	<h5>Task 3: Escalate to Administrative Privileges</h5>
	<i>It looks like the design of the authentication system is very flawed. Find out how users are authenticated and exploit the system to gain Administrator rights.</i><br>
	Check what the task asks one more time. <u>Find out how users are authenticated</u>. It is time to intercept the login request with Burp.<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/3_admin_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Pretty clear, the cookie can be read as plain text and there is a field about the admin privileges of the logged in account. Which can be edited. To Admin. True. Kudos to the Mayor's Blog sysadmin (spoiler alert for the next task??).
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/3_logged_as_admin_marked.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	And this is the story of how I became Administrator of the Mayor's blog. Oh, cool, I can even see some nginx logs from here.<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/3_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	And a flag?<br>
	<h5>Task 4: Gain Access to the Developer Console</h5>
	<i>There is a special console for administrators on the website, but the link is hidden. Find the link, and gain relevant information from the logs. In the end, use the console to find user information.</i><br>
	So, what happened here is that, the sysadmin thought,<i> "I have a shell with admin privileges stored in here, just in case I will need it??, but I don't want to be crawled, I need to hide it. Let's add it to a robots.txt"</i>.<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/4_robots.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	I will be fair with our sysadmin here, even though the admin_shell was not, really, hidden, it couldn't be accessed even by altering the cookie to admin. I had to find a way arround to this. So, lets take a step back and check these logs we discovered before. And I mean, thorough check (I spent some time trying to make my way with an SSRF exploit. Time wasted? never).<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/4_sysadmin.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Is that the sysadmin's account password? In plain text again? Didn't you have one job?<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/4_logged_as_sysadmin.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	Climbing to the top of this blog. Do you believe that now I gained access to the admin_shell?<br>
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/4_flag.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	With ls and cat. Making my life simple.<br>
	<h5>Task 5: Crack the Password of the Admin Account on the Server</h5>
	<i>Find a vulnerability in the developer console and use it to access a password file.</i><br>
	For this task I didn't manage to find the flag. But, here's what I have.
	<img src="assets/images/ctfs/hacky_holidays/mayor's_blog/5_pass_shad.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
	I tried some combinations of piping and merging commands with the ';' operator, but it was way easier. With the cat command we could traverse back from the current path to any other. That didn't work with the ls command. I managed to retrieve the shadow and passwd files from the server file system, but I couldn't break them with john. And I tried. So many times U+1F979. Maybe the answer was something else, or I didn't set john right, anyways, here's the vulnerability, or a vulnerability.
</article><br><br>
<article>
<h3><b>You Can't See Me | Network, ICS | Easy</b></h3>
</article>
