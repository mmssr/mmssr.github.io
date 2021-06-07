---
layout: post
title: "CTF Challenge: Forwards From Grandma"
date: 2021-06-07
---

<h2>A Brief Introduction to CTFs</h2>  
CTFs are information security competitions which tend to come in a few formats. Most that I have participated in follow a "Jeopardy Board" format, with numerous challenges to be solved. Others are more styled like hack-the-box, where you have a machine to root, and so on. Either way, the goal is to capture a flag, which is often a string in the format of flag{blah_blah_blah}. These are fantastic competitions for a variety of reasons, such as improving your technical skillset, learning to use new tools, and gaining a methodology for approaching abstract problems with little to no hints or prompts.  

<h2>Forwards From Grandma</h2>  
This was a challenge that I found from the Tenable CTF in 2021. You are given an <a href:="https://github.com/mmssr/mmssr.github.io/blob/master/assets/tmp.eml">email file</a> along with the prompt: "My grandma sent me this email, but it looks like there’s a hidden message in it. Can you help me figure it out?" Feel free to poke at it yourself, but don't scroll too far as I will be spoiling it. Assuming you aren't using some bizarre OS I have never heard of, this challenge should be solvable without any special tools.  

Let's take a look at the email:  
![email](/assets/fwdfrmgma.PNG)  

Nothing sticks out too much, huh? I attempted to run some stego tools on the image, but that didn't seem to mean much. Next step was to open the email in a text editor:  
```  

MIME-Version: 1.0
Date: Thu, 14 Jan 2021 11:16:51 -0500
Message-ID: <CAC=sLTtXz8LtqcvXeC5fmqVEze+hqHNOWK=40+A8e3CSr_-pw@mail.gmail.com>
Subject: FWD: FWD: RE: FWD:  FWD: RE: FWD: FWD:  FWD: RE:  RE: RE: FWD: { FWD:
 FWD:  FWD: FWD: RE: RE: FWD: RE:  RE: RE:  FWD: FWD:  FWD: FWD: FWD:  FWD:
 FWD: FWD:  FWD: FWD: RE: RE: FWD: RE:  FWD: RE:  RE: RE: RE:  FWD: RE: FWD:
 FWD: } THIS IS HILARIOUS AND SO TRUE
From: Grandma <grannyG@gmail.com>
To: "edna" <edna@gmail.com>
Content-Type: multipart/mixed; boundary="0000000000006332fa05b8de95db"

--0000000000006332fa05b8de95db
Content-Type: multipart/alternative; boundary="0000000000006332f805b8de95d9"

--0000000000006332f805b8de95d9
Content-Type: text/plain; charset="UTF-8"

thinking of you

--grandma

--0000000000006332f805b8de95d9
Content-Type: text/html; charset="UTF-8"

<div dir="ltr">thinking of you<div><br></div><div>--grandma</div><div><br></div></div>

--0000000000006332f805b8de95d9--
--0000000000006332fa05b8de95db
Content-Type: image/jpeg; name="ffg.jpg"
Content-Disposition: attachment; filename="ffg.jpg"
Content-Transfer-Encoding: base64
X-Attachment-Id: f_kjx1wv5o0
Content-ID: <f_kjx1wv5o0>

/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAgGBgcGBQgHBwcJCQgKDBQNDAsLDBkSEw8UHRofHh0a
HBwgJC4nICIsIxwcKDcpLDAxNDQ0Hyc5PTgyPC4zNDL/2wBDAQkJCQwLDBgNDRgyIRwhMjIyMjIy
MjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjL/wAARCAIYAcIDASIA
AhEBAxEB/8QAHAAAAQUBAQEAAAAAAAAAAAAAAAMEBQYHAgEI/8QAXxAAAQMCBAQEAgUGBwsHCQgD
AQIDBAURAAYSIQcTMUEUIlFhI3EVJDKBkQgWMzRCoRg1UmKx0fAXJTZDU2N0k7KzwSZEVnJ1kuEn
N0Zkc4LS4/FUVZSio7TCw9Nlg//EABQBAQAAAAAAAAAAAAAAAAAAAAD/xAAUEQEAAAAAAAAAAAAA

```  
There were about 1700 more lines to this, but nothing particularly stuck out to me yet. Ctl+F revealed nothing for "flag{", which was the flag format. However, there were some curly braces that stuck out like a sore thumb in the subject: ```FWD: FWD: RE: FWD:  FWD: RE: FWD: FWD:  FWD: RE:  RE: RE: FWD: { FWD:  FWD:  FWD: FWD: RE: RE: FWD: RE:  RE: RE:  FWD: FWD:  FWD: FWD: FWD:  FWD:  FWD: FWD:  FWD: FWD: RE: RE: FWD: RE:  FWD: RE:  RE: RE: RE:  FWD: RE: FWD:  FWD: } THIS IS HILARIOUS AND SO TRUE``` 

So what can we do with this? I tried about a billion different things. What I first began to believe is that perhaps everything from before the first curly brace equated "flag" somehow. Next I noticed odd spacing going on, perhaps indicating things were broken up into words. After playing about with thinking maybe FWD and RE mapped to 0 and 1, I realized they likely did... in morse code. Sure enough, ```..-. .-.. .- --.``` translates to flag. The rest of the challenge was pretty easy, and you get ```..-. .-.. .- --. { ..  -- .. ... ...  .- --- .-..}```, giving us the final output: ```flag{i_miss_aol}```.
