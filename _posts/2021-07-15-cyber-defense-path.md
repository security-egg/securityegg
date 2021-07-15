---
layout: post
title: "TryHackMe: Cyber Defense Learning Path P1D4"
date: 2021-07-15
categories: tryhackme
---

Leaving networking aside for the next two rooms, we will get a taste of the most popular home OS. Windows. The first room will introduce as the general aspects of the OS 
,while with the second we will dive in to the administrative side of it, the Active Directory.<br>
I will skip the first room (:O oh,yes). For the first 5 tasks you just have to read all the information included, the answers are in there. Read carefully, a lot of interesting things are 
referred there.<br>
For the last two tasks we have to fire up the machine and follow the given instructions. The title "Intro to Windows" fits well to this first impression (you don't say?). <br><br>

Lets move on to the second room. The first task is, again, introductory. Until task 8 we continue with the reading and question answering.

<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h3>Active Directory Basics</h3>
<p style="margin-top: 2rem">
<h5><b>Task 8</b></h5>
Deploy the machine and a Windows Server will appear at the right(give it some time before it's fully loaded).
<ul style="margin-top: 2rem">
    <li style="margin-top: 1rem">
     <i><b>What is the name of the Windows 10 operating system?</b></i><br>
	Through a command prompt, follow the instructions provided before in the task. Type <i>Get-NetComputer -fulldata | select operatingsystem</i> to get the answer.
    </li>
    <li style="margin-top: 1rem">
     <i><b>What is the second "Admin" name?</b></i><br>
	Still going on with the instructions, try <i>Get-NetUser | select cn</i> and look for another Admin name than the Administrator.
    </li>
    <li style="margin-top: 1rem">
     <i><b>Which group has a capital "V" in the group name?</b></i><br>
	We have to search further in the cheatsheet (check the hint only if you want to save time). You need something like Get-...Group with a specified option.
    </li>
    <li style="margin-top: 1rem">
     <i><b>When was the password last set for the SQLService user?</b></i><br>
	You are looking for smthing like Get-...User, an option and pipe that to filter the results (ok, now check the hint if you want to, I did).
    </li>
</ul>
</p>
</article>

That was all with the Windows rooms and the first part. Hooray! Our Cyber Defense path journey started cool and easy. Personally, I want to learn more!
