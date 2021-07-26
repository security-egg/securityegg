---
layout: post
title: "TryHackMe: Biohazard"
date: 2021-07-26
categories: tryhackme
---

As a RE series fan, I could not resist the tempt of joining the Biohazard room. The room is stated as medium, but I would say it belongs to the easiest of the medium ranks. Leaving aside the toughness and the theme, the room has a nice way to lead
you with its foothold. The challenges you will find are mostly of the stego and base decoding categories, as long as with some looking at the source of the web pages.


<article style="margin-top: 2%; padding: 1%; border-left: 2px solid">
<h5><b>Task 1: Introduction</b></h5>
<p style="margin-top: 2rem">
We start with the usual enumeration.<br>
<img src="/securityegg/assets/images/tryhackme/biohazard/nmap_enum.jpg" alt="#" style="margin-top: 2%; max-width: 100%"><br>
From the nmap scan we can see that there are 3 open ports, the usual 22 with an ssh service and the 80 with an apache service running on them. There is also a ftp service running on port 21. Moving foward, we can check the webpage from the browser.<br>
Read the (nostalgic for those who know the story of RE 1) note and click on the mansion... If you dare :D.
</p>
<h5><b>Task 2: The Mansion</b></h5>
<p style="margin-top: 2rem">
The room follows exactly the game's pace. So, getting on with the foothold is really funny. We start from the Mansion and, specifically, from the Main Hall. Friendly advice, usually, you are safe from zombies in the Main Hall...usually. <br>
Checking the source, we see a forgotten comment that directs us to the /diningRoom.Navigate to the directory and you will see a new page, the Dining Room.<br>
Read the text, get the <u>emblem flag</u> and check the source again. There is another comment, this time encoded in base64. Do not submit the emblem flag, is not the right one for this slot. We move foward to the /teaRoom, as we found out from by decoding the comment.
Before changing the directory, I suggest you to search in youtube for the clip from resident evil 1 first zombie. Ok, ok, we go to the /teaRoom and Barry is so kind to give us the <u>lockpick flag</u>. Take it and move foward to the /artRoom.
After all, I am in the happy place to inform you that...we got the map! Hooray and the Great Gatsby should be placed here, anyway...let's check all the other rooms. Next is the /barRoom.<br>
The door opens with the lockpick flag and we get another note. We music note is encoded with base32 so, decode it and you will get the <u>music sheet flag</u>. Submit the flag to play the piano.
Yes, we have the <u>gold emblem</u>! Take it and submit in its place the previous emblem we found (just RE stuff). We get a name, rebecca, keep that in mind, we will need it after.</br>
Let's return to the /diningRoom and submit the gold emblem flag in the emblem slot. We get a cipher, not a base encoding this time. It looks like a Vigenere and we have a possible key...Yes, rebecca deciphers this Vigenere cipher and we get the directions for the_great_shield_key.html page, inside the /diningRoom. Navigate to there and get the <u>shield key</u>.
Now, let's check the rest of the rooms. There is one more view of the Dining Room at the second floor. Go to /diningRoom2F.<br>
Help Jill get the blue gemstone. Look at he source, there is a cipher. It looks like a Vigenere cipher again, but it's not. Its a simple Caesar cipher. You can brute force it with the help of dcode.fr and get the answer. Next stop, the sapphire.html inside the /diningRoom (remember, the gem stone felt from the second floor). Get the <u>blue jewel flag</u>.<br>
I will head to the /tigerStatusRoom where is a slot for the jewel. Submit the flag and you will see something new... We get the fist crest that is needed to decode the ftp username and password. I will come back to this soon.<br>
Next room is the /galleryRoom. We have to examine it and get the second crest. Nice. <br>
For the /studyRoom we need the helmet flag. Leave it for last. Move to the /armorRoom. To enter, we need the shield flag. Submit it, read the note and get the third crest. One more to go...<br>
To the /attic, once more we submit the shield flag and get in to find the final crest. It is time to combine them and get the username and password.<br>
So, read the notes and notice four things: i) the encoded text, ii) how many times the plaintext was encoded, iii) how many letters the plaintext containes and, iv) that the text was encoded, not encrypted, which means that we are looking to decode it with a combination of baseXX encodings. From the other clues, I suppose we can find which encoding matches everytime by checking the symbols of the encoded text and compairing the length with the length of the plaintext plus the expected padding. I tried to figure it out like so, but I was pretty tired at that time so I just tried some recipes on CyberChef. Finally, I managed to decode it as this:
<ul>
<li>
crest1: base64 -> base32
</li>
<li>
crest2: base32 -> base64
</li>
<li>
crest3: base64 -> binary -> hex
</li>
<li>
crest4: base58 -> hex
</li>
<ul>
Combine the outputs to one string and decode it with base64. You will get the ftp username and password.
</p>
<h5><b>Task 3: The guard house</b></h5>
<p style="margin-top: 2rem">
We escaped the mansion but the terror has not ended. We need more keys to get ourselves free from this horror. Luckily, on the ftp server we can find enough of them (xD).<br>
<img src="/securityegg/assets/images/tryhackme/biohazard/ftp.jpg" alt="#" style="margin-top: 2%; max-width: 100%"><br>
Everything seems interesting so, let's get them all. Starting with the important.txt, we find another hidden room, the /hidden_closet. Barry references that the helmet key is needed and, also, that it is encrypted and he has no clue on how to solve it. We can check it out and help Barry.<br>
The key is inside the helmet_key.txt.gpg, which can't be encrypted without the passphrase. There are 3 key pictures that could be very helpful. They seem a little...stegoed?<br>
For the first, try steghide and you will get a .txt.
The second has a comment visible with exiftool.
Finally, the third hides another .txt inside it, which can be extracted with binwalk. Combine all 3 and decode the text with base64. This is the passphrase for the .gpg.<br>
Try gpg --decrypt helmet_key_.txt.gpg, give the password and you will get the helmet key.<br>
</p>
<h5><b>Task 4: The revisit</b></h5>
<p style="margin-top: 2rem">
Back to the /hidden_closet, the door was hiding an underground cave (who could tell?). There we find Enrico, the only survivor and leader of the STARS. He gives us a MO disk and a wolf medal.<br>
The MO disk is, one more time, encrypted with the Vigenere cipher. It can be brute forced, the key is Albert, and we find Weasker's login password (how unkind of you Weasker). The wolf medal gives us the ssh password, but for which username?<br>
I had to press the hint for this one and I got informed that I had forgotten a room. Facepalm.<br>
Return to the Mansion and navigate to the /studyRoom. If you remember, the room needed the helmet flag to open. Open the room and examine the book. You will get a .tar.gz. From that, we get the an eagle_medal.txt with the ssh username.
</p>
<h5><b>Task 5: Underground laboratory</b></h5>
<p style="margin-top: 2rem">
In the last task we find a hidden, underground laboratory (oh, no, they make zombies here). As an umbrella guest, we find a curious hidden dir.<br>
<img src="/securityegg/assets/images/tryhackme/biohazard/ssh.jpg" alt="#" style="margin-top: 2%; max-width: 100%"><br>
Go to .jailcell to find chris.txt. And Chris, as well...<br>
Previously, we brute forced the Vigenere ciphertext inside the MO disk and found out Weasker's password. Now it's time to put it on test. Change user to weasker, give the password, check the permissions and get the root flag from the /root dir.<br>
<img src="/securityegg/assets/images/tryhackme/biohazard/weasker.jpg" alt="#" style="margin-top: 2%; max-width: 100%"><br> 
And then run. Fast. Cause Weasker will possibly transform into a Tyrant. You never know.
</p>
