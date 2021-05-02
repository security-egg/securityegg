---
layout: post
title: "TryHackMe: Cyber Defense Learning Path P1D2"
date: 2021-05-02
categories: tryhackme
---

Moving foward with the Cyber Defense path, the next two rooms are all about Network Services. Here, we will see how protocols and services can be exploited. The goal for this day is 
to complete both rooms.
The first room introduces us with three well known protocols, the SMB, Telnet and FTP, while the second room continues with the NFS and SMTP protocol, as long as with MySQL.

<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h3>Network Services</h3>
<p style="margin-top: 2rem">
<h5><b>Task 2</b></h5>
Say your computer needs to communicate with a server. With the SMB protocol, it will establish a connection with the server while trading a number of messages. It may be sound extremelly 
usefull, but why don't you check an <a href="https://nordvpn.com/blog/what-is-smb/">article</a> about how vulnerable has been proved.
  <ul style="margin-top: 2rem">
    <li>
     <i><b>What does SMB stand for?</b></i><br>
	A protocol by which your computer exchanges messages with a server. A <u>Server Message Block</u> protocol.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What type of protocol is SMB?</b></i><br>
	Computer and server will continuously communicating with response and request messages. The answer is response-request.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What do clients connect to servers using?</b></i><br>
	Try to remember what you learned from the previous room. With SMB, two machines are establishing a session so that they transfer data. They are using TCP/IP.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What systems does Samba run on?</b></i><br>
	Who could say, <u>Unix</u> have a taste for Brazilian music.
    </li>
  </ul>
<h5><b>Task 3</b></h5>
Before trying to exploit, gather some information. This process is called enumeration and in this task we will enumerate the SMB.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Conduct an nmap scan of your choosing, How many ports are open?</b></i><br>
	Bring it on, nmap! I can clearly see 3 open ports.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What ports is SMB running on?</b></i><br>
	Two default ports where SMB runs, 139/445 is the right answer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?</b></i><br>
	Oh, the irony. WORKGROUP.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What comes up as the name of the machine?</b></i><br>
	Check through the results to find POLOSMB.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What operating system version is running?</b></i><br>
	6.1
    </li>
    <li style="margin-top: 1rem">
     <i><b>What share sticks out as something we might want to investigate?</b></i><br>
	Don't you think, <u>profiles</u> seems like an interesting investigation option.
    </li>
  </ul>
<h5><b>Task 4</b></h5>
Exploitation time! We have already gathered some information about our target and we are ready to try some attacks on it.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?</b></i><br>
	To access an SMB share you need to run an SMB client. The tool we'll use is called <u>smbclient</u>. Our target's IP is <u>10.10.10.2</u>. 
	We want to take a look of <u>secret</u>'s contents as the user <u>suit</u>. Also, SMB's service runs on port <u>445</u>.<br>
	smbclient //10.10.10.2/secret -U suit -p 445
    </li>
    <li style="margin-top: 1rem">
     <i><b>Does the share allow anonymous access? Y/N?</b></i><br>
	Actually by a missconfiguration and not a vulnerability, this share <u>allows</u> anonymous access.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?</b></i><br>
	Take a look around to find mr. <u>John Cactus</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What service has been configured to allow him to work from home?</b></i><br>
	It's really common for professionals to use <u>SSH</u> as a work-from-home solution.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Okay! Now we know this, what directory on the share should we look in?</b></i><br>
	Note down <u>.ssh</u>, you will meet again with this directory.
    </li>
    <li style="margin-top: 1rem">
     <i><b>This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?</b></i><br>
	An <u>id_rsa</u> key file for free.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the smb.txt flag?</b></i><br>
	SSH keys and privilege escalation. Finding the flag is now a piece of cake.<br>THM{smb_is_fun_eh?}
    </li>
  </ul>
<h5><b>Task 5</b></h5>
Lack of encryption does not fit to modern networks. That's why Telnet got replaced by SSH.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is Telnet? </b></i><br>
	Telnet is also a protocol and, actually, an <u>application protocol</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What has slowly replaced Telnet?</b></i><br>
	The way more trustful <u>SSH</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?</b></i><br>
	You need a <u>telnet</u> command line tool to connect with the IP <u>10.10.10.3</u> via port <u>23</u>.<br>
	telnet 10.10.10.3 23
    </li>
    <li style="margin-top: 1rem">
     <i><b>The lack of what, means that all Telnet communication is in plaintext?</b></i><br>
	Without <u>encryption</u> do not expect any ciphertexts.
    </li>
  </ul>
<h5><b>Task 6</b></h5>
Lets see what can we find in here.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>How many ports are open on the target machine?</b></i><br>
	Nmap says 1 port is open.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What port is this?</b></i><br>
	And the port number is... 8012.
    </li>
    <li style="margin-top: 1rem">
     <i><b>This port is unassigned, but still lists the protocol it's using, what protocol is this?</b></i><br>
	At least we know it runs on <u>TCP</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Now re-run the nmap scan, without the -p- tag, how many ports show up as open?</b></i><br>
	0 of the most popular ports.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Based on the title returned to us, what do we think this port could be used for?</b></i><br>
	Oh no, it's <u>a backdoor</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Who could it belong to? Gathering possible usernames is an important step in enumeration.</b></i><br>
	I guess, <u>Skidy</u> placed the backdoor in there.
    </li>
  </ul>
<h5><b>Task 7</b></h5>
Now we become aggressive.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Great! It's an open telnet connection! What welcome message do we receive?</b></i><br>
	SKIDY'S BACKDOOR, how obvious.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)</b></i><br>
	Sadly, or not, N.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Now, use the command "ping [local THM ip] -c 1" through the telnet session to see if we're able to execute system commands. 
	Do we receive any pings? Note, you need to preface this with .RUN (Y/N)</b></i><br>
	Y, we have pings.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What word does the generated payload start with?</b></i><br>
	mkfifo
    </li>
    <li style="margin-top: 1rem">
     <i><b>What would the command look like for the listening port we selected in our payload?</b></i><br>
	Previously, we set the port 4444 as the one we want to listen to. You have to set netcat to listen on this port.<br>
	nc -lvp 4444
    </li>
    <li style="margin-top: 1rem">
     <i><b>Success! What is the contents of flag.txt?</b></i><br>
	Wasn't that hard, right?<br> THM{y0u_g0t_th3_t3ln3t_fl4g}
    </li>
  </ul>
<h5><b>Task 8</b></h5>
You will encounter this protocol often. FTP is one of the most preferred ways for file transfering over a network.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What communications model does FTP use?</b></i><br>
	Once more, is the <u>client-server</u> model.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What's the standard FTP port?</b></i><br>
	21
    </li>
    <li style="margin-top: 1rem">
     <i><b>How many modes of FTP connection are there? </b></i><br>
	We have Active and Passive FTP connection. The answer is 2.
    </li>
  </ul>
<h5><b>Task 9</b></h5>
Nothing new here, just use nmap to gather intelligence.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>How many ports are open on the target machine? </b></i><br>
	2
    </li>
    <li style="margin-top: 1rem">
     <i><b>What port is ftp running on?</b></i><br>
	On it's default port, 21.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What variant of FTP is running on it?</b></i><br>
	The <u>vsftpd</u>, the default FTP server for Linux.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the name of the file in the anonymous FTP directory?</b></i><br>
	PUBLIC_NOTICE.txt
    </li>
    <li style="margin-top: 1rem">
     <i><b>What do we think a possible username could be?</b></i><br>
	Why not <u>mike</u>?
    </li>
  </ul>
<h5><b>Task 10</b></h5>
Action!
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the password for the user "mike"?</b></i><br>
	Mike should have tried something harder than just <u>password</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is ftp.txt?</b></i><br>
	THM{y0u_g0t_th3_ftp_fl4g}
    </li>
  </ul>
</p>
</article>

<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h3>Network Services 2</h3>
<p style="margin-top: 2rem">
<h5><b>Task 2</b></h5>
One more protocol that allows us to transfer files, you may have noticed NFS as an option of an Android device.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What does NFS stand for?</b></i><br>
	Network File System
    </li>
    <li style="margin-top: 1rem">
     <i><b>What process allows an NFS client to interact with a remote directory as though it was a physical device?</b></i><br>
	In order to accomplish the directory and file transfers, NFS mounts at least a portion of a file system on a server. This process is known as <u>mounting</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What does NFS use to represent files and directories on the server?</b></i><br>
	If you wanted to access a file, the NFS would return to you a unique identifier of it, meaning a <u>file handle</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What protocol does NFS use to communicate between the server and client?</b></i><br>
	One more extremely helpful protocol, <u>RPC</u>, standing for Remote Procedure Call, offers functionality when we have to call a procedure on a remote computer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What two pieces of user data does the NFS server take as parameters for controlling user permissions? Format: parameter 1 / parameter 2</b></i><br>
	Two things that define a user, it's <u>user id</u> and the <u>group id</u> the user belongs to.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Can a Windows NFS server share files with a Linux client? (Y/N)</b></i><br>
	Yes, of course.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Can a Linux NFS server share files with a MacOS client? (Y/N)</b></i><br>
	Yes, again.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the latest version of NFS? [released in 2016, but is still up to date as of 2020] This will require external research.</b></i><br>
	Good old friend, google. The answer is 4.2.
    </li>
  </ul>
<h5><b>Task 3</b></h5>
Here we go again. You now know how enumartion goes.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Conduct a thorough port scan scan of your choosing, how many ports are open?</b></i><br>
	Some more ports than usual. I can see 7 open ports.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which port contains the service we're looking to enumerate?</b></i><br>
	We are looking for NFS. It's running on port 2049.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Now, use /usr/sbin/showmount -e [IP] to list the NFS shares, what is the name of the visible share?</b></i><br>
	/home sweet /home.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Then, use the mount command we broke down earlier to mount the NFS share to your local machine. Change directory to where you mounted the share- what is the name of the folder inside?</b></i><br>
	Close to a favorite Italian coffee, the answer is cappucino.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Interesting! Let's do a bit of research now, have a look through the folders. Which of these folders could contain keys that would give us remote access to the server?</b></i><br>
	Remember what I told you about the <u>.ssh</u> dir?
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which of these keys is most useful to us?</b></i><br>
	You got it, <u>id_rsa</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Can we log into the machine using ssh -i {key-file} {username}@{ip} ? (Y/N)</b></i><br>
	Same procedure, Yes, we can.
    </li>
  </ul>
<h5><b>Task 4</b></h5>
Moving on to the next phase.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>Now, we're going to add the SUID bit permission to the bash executable we just copied to the share using "sudo chmod +[permission] bash". 
	What letter do we use to set the SUID bit set using chmod?</b></i><br>
	SUID bit, sudo, <u>s</u> is the answer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Let's do a sanity check, let's check the permissions of the "bash" executable using "ls -la bash". 
	What does the permission set look like? Make sure that it ends with -sr-x.</b></i><br>
	It should look like <u>-rwsr-sr-x</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great! If all's gone well you should have a shell as root! What's the root flag?</b></i><br>
	THM{nfs_got_pwned}
    </li>
  </ul>
<h5><b>Task 5</b></h5>
The "Simple Mail Transfer Protocol" is one of those responsible for transfering emails through networks.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What does SMTP stand for?</b></i><br>
	Easy to remember, <u>SImple Mail Transfer Protocol</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What does SMTP handle the sending of?</b></i><br>
	What else than <u>emails</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the first step in the SMTP process?</b></i><br>
	<u>SMTP handshake</u>'s the best way for an email to start it's journey.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the default SMTP port?</b></i><br>
	Google it. The answer is 25.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Where does the SMTP server send the email if the recipient's server is not available?</b></i><br>
	If you were an email, you would have to wait at a <u>smtp queue</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>On what server does the Email ultimately end up on?</b></i><br>
	POP/IMAP
    </li>
    <li style="margin-top: 1rem">
     <i><b>Can a Linux machine run an SMTP server? (Y/N)</b></i><br>
	Why not? The correct answer is Y.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Can a Windows machine run an SMTP server? (Y/N)</b></i><br>
	Same as before, Yes, it can.
    </li>
  </ul>
<h5><b>Task 6</b></h5>
Start your machine!
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>First, lets run a port scan against the target machine, same as last time. What port is SMTP running on?</b></i><br>
	Do you remember? Port 25.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Okay, now we know what port we should be targeting, let's start up Metasploit. What command do we use to do this?</b></i><br>
	Metasploit starts with the <u>msfconsole</u> command.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Let's search for the module "smtp_version", what's it's full module name?</b></i><br>
	You need to "search smtp_version". There's only one response after, auxiliary/scanner/smtp/smtp_version.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great, now- select the module and list the options. How do we do this?</b></i><br>
	Type use and the module's name to select it. Type <u>options</u> to list the available options.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Have a look through the options, does everything seem correct? What is the option we need to set?</b></i><br>
	Almost everything is ready, you just have to set the <u>RHOSTS</u> with the target's IP address.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Set that to the correct value for your target machine. Then run the exploit. What's the system mail name?</b></i><br>
	polosmtp.home
    </li>
    <li style="margin-top: 1rem">
     <i><b>What Mail Transfer Agent (MTA) is running the SMTP server? This will require some external research.</b></i><br>
	Postfix
    </li>
    <li style="margin-top: 1rem">
     <i><b>Good! We've now got a good amount of information on the target system to move onto the next stage. Let's search for the module "smtp_enum", what's it's full module name?</b></i><br>
	Search again. The answer is auxiliary/scanner/smtp/smtp_enum.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What option do we need to set to the wordlist's path?</b></i><br>
	A different module requires different setups. USER_FILE, first.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Once we've set this option, what is the other essential paramater we need to set?</b></i><br>
	After that, RHOSTS.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Okay! Now that's finished, what username is returned?</b></i><br>
	We got the <u>administrator</u>.
    </li>
  </ul>
<h5><b>Task 7</b></h5>
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the password of the user we found during our enumeration stage?</b></i><br>
	Brute force the authentication once more. The answer is alejandro.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great! Now, let's SSH into the server as the user, what is contents of smtp.txt</b></i><br>
	THM{who_knew_email_servers_were_c00l?}
    </li>
  </ul>
<h5><b>Task 8</b></h5>
Not just protocols can be exploited.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What type of software is MySQL?</b></i><br>
	Important to know, MySQL is a <u>relational database management system</u> software.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What language is MySQL based on?</b></i><br>
	Easy, SQL.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What communication model does MySQL use?</b></i><br>
	Typical, <u>client-server</u> model.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is a common application of MySQL?</b></i><br>
	Hidding behind e-shops, a <u>back end database</u>.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What major social network uses MySQL as their back-end database? This will require further research.</b></i><br>
	Some researching, or just read the hint, Facebook is the correct answer.
    </li>
  </ul>
<h5><b>Task 9</b></h5>
Lets see what we can find.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>As always, let's start out with a port scan, so we know what port the service we're trying to attack is running on. What port is MySQL using?</b></i><br>
	The default port, 3306.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Search for, select and list the options it needs. What three options do we need to set? (in descending order).</b></i><br>
	PASSWORD/RHOSTS/USERNAME
    </li>
    <li style="margin-top: 1rem">
     <i><b>Run the exploit. By default it will test with the "select version()" command, what result does this give you?</b></i><br>
	5.7.29-0ubuntu0.18.04.1
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great! We know that our exploit is landing as planned. Let's try to gain some more ambitious information. 
	Change the "sql" option to "show databases". how many databases are returned?</b></i><br>
	4 databases.
    </li>
  </ul>
<h5><b>Task 9</b></h5>
Type msfconsole and hit enter for this part.
  <ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>First, let's search for and select the "mysql_schemadump" module. What's the module's full name?</b></i><br>
	auxiliary/scanner/mysql/mysql_schemadump
    </li>
    <li style="margin-top: 1rem">
     <i><b>Great! Now, you've done this a few times by now so I'll let you take it from here. Set the relevant options, run the exploit. 
	What's the name of the last table that gets dumped?</b></i><br>
	Strange name for a table? x$waits_global_by_latency
    </li>
    <li style="margin-top: 1rem">
     <i><b>Awesome, you have now dumped the tables, and column names of the whole database. 
	But we can do one better... search for and select the "mysql_hashdump" module. What's the module's full name?</b></i><br>
	How many modules. This time use auxiliary/scanner/mysql/mysql_hashdump.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Again, I'll let you take it from here. Set the relevant options, run the exploit. What non-default user stands out to you?</b></i><br>
	carl is definitely a non-default user.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the user/hash combination string?</b></i><br>
	Just copy-paste it. carl:*EA031893AA21444B170FC2162A56978B8CEECE18
    </li>
    <li style="margin-top: 1rem">
     <i><b>Now, we need to crack the password! Let's try John the Ripper against it using: "john hash.txt" what is the password of the user we found?</b></i><br>
	Time for some cracking. The answer is doggie.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What's the contents of MySQL.txt</b></i><br>
	You have everything you need to find the flag. THM{congratulations_you_got_the_mySQL_flag}
    </li>
  </ul>
</p>
</article>

And with that, two rooms about Network Services were completed!
