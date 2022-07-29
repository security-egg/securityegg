---
layout: post
title: "Cyberdefenders - GrabThePhisher"
date: 2022-07-29
categories: cyberdefenders
---

Moving on to the blue side, under the Cyberdefenders category I will add some writeups from the Cyberdefenders' challenges. The first one I have for you is the 'GrabThePhisher'. Lets read the description.<br>
<i>An attacker compromised a server and impersonated hxxps://pancakeswap.finance/, a decentralized exchange native to BNB Chain, to host a phishing kit at hxxps://apankewk[.]soup[.]xyz/mainpage.php. The attacker set it as an open directory with the file name "pankewk.zip".Provided the phishing kit, you are requested to analyze it and do your threat intel homework.</i><br>

So, we have an attacker which compromised a server. A phishing kit, somewhere hidden in the downloadable challenge, we'll get to this. And, we are asked to perform some threat intel on it. Time to make a dive.<br><br>

Before heading to the challenge questions I like to explore the challenge's content, just to get an overall view of what I have in hand and understand the scenario. So, what we will see in the package, and is meaningfull to start our investigation from there, are, 1) some jpgs giving us a taste of the original web app, prior to its compromisation, 2) the <i>"index.html"</i> file, which can be translated as "the home page of the web page", the one to be shown if you navigate to the domain via a browser, 3) a log directory and 4) an src directory.<br>

Under the src directory you will find many many functional files. In order to understand where everyone is useful, and if we need to examine them more, we will start from the <i>index.html</i>, just to figure out how this app operates. Before jumping there, under the log dir we see a <i>log.txt</i> with some strange entries. Leave it for later and return to the index. You may open it on the browser to see a graphical representation, but we mostly care about it's source code. From what we have, this app is about connecting a crypto wallet. To find how, scroll right to the bottom of the file.<br><br>

<article>
<h5>Question 1: Which wallet is used for asking the seed phrase?</h5><br>
Now, we see two pieces of code inside script tags. The first one describes the functionality of the Metamask wallet. You can observe that the same code has not been developed for any other crypto wallet between those available on the app.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/1_index_connect.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
Furthermore, the second script is about counting words of a secret recovery phrase.<br><br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/1_secret_phrases.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
This has to be our answer and our next step.<br>
<h5>Question 2: What is the file name that has the code for the phishing kit?</h5><br>
Back to our app's structure, we see a <i>metamask</i> directory, with another <i>index.php</i> and a <i>metamasks.php</i> file.<br>
<h5>Question 3: In which language was the kit written?</h5><br>
Open them both, they're will be needed. The <i>metamasks.php</i>, indeed, has some php code (what? you never know until you see).<br>
<h5>Question 4: What service does the kit use to retrieve the victim's machine information?</h5><br>
Top to bottom, we examine the code. First, we see a variable named <i>request</i>. It is implemented to get the contents of an api, along with a IP address.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/4_api.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
We are not supposed to be aware of every api hosted online but, we can always google it and the word geo seems to reference to a geolocation api. As expected, google did it's job and we discover the existence of the <i>Sypex Geo</i> api.<br>
<h5>Question 5: How many seed phrases were already collected?</h5><br>
The last part of the file is about a function called <i>sendTel</i> which, takes the contents of the <i>message</i> variable, defined previously, creates two new variables, the <i>id</i> and <i>token</i>, and then, creates a url with these three variables to <i>api.telegram</i>. After that, gets the contents of the url's request POST answer, only the data part, and appends it to the file <i>log.txt</i>. The one with the strange contents from before?<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/5_code_part.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br><br>
Lets return to this file and see its contents.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/5_seeds.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
We see words splitted to three lines. Now remember, we discovered two code parts referring to the seed phrases. The one was counting the words, so we expect to see a specific number of words, and the other was appending the phrase to the file. Appending with the FILE_APPEND argument. Meaning, every line is a seed phrase.
<h5>Question 6: Write down the seed phrase of the most recent phishing incident?</h5><br>
And, the new lines are appending at the end of the file.
<h5>Question 7: Which medium had been used for credential dumping?</h5><br>
But, what is the meaning of the request to that Telegram api. Read the url one more time.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/7_code_part.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
After constructing a message, containing the secret phrase, the IP and geolocation info and the username, it appends it to that request. The request is made to the <i>/sendMessage?</i> uri, with a specific chat ID of a bot with a specific token. Meaning, our attacker steals that info from the Metamask users of our application and then forwards them to a personal chat of that Telegram api.
<h5>Question 8: What is the token for the channel?</h5><br>
We know the token.<br>
<h5>Question 9: What is the chat ID of the phisher's channel?</h5><br>
We also know the id.<br>
<h5>Question 10: What are the allies of the phish kit developer?</h5><br>
There is one part of the code we didn't pay attention yet. It's just a comment, a small gift the phisher gave to us. Why? To mock us? We will prove better.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/10_allies.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
<h5>Question 11: What is the full name of the Phish Actor?</h5><br>
You see, our attacker had the attitude to leave a, propably, personal information behind. Don't expect to get anything from that because, clearly, the phisher knew that, leaving such a trace would make no harm. But, the trace that can finally lead the path towards discovering the attacker is the chat where the seeds get dumped. Lets recreate it, with a hello message this time just to laugh back.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/recreate_link.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
Everything in place and we only need to try this.<br>
<img src="/securityegg/assets/images/cyberdefenders/phisher/11_12_answers.png" alt="oh, no!" style="margin-top: 2%; max-width: 100%"><br>
We have both the full name and the username for the next question. The investigation is complete :D.
</article>
