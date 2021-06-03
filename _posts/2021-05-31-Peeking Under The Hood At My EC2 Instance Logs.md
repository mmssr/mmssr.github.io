---
layout: post
title: "Peeking Under The Hood At My EC2 Instance Logs"
date: 2021-05-31
---

<h2>Playing around in AWS for fun <s>and profit</s></h2>  
Setting up an Amazon Web Services EC2 instance is a good time. EC2 instances are available through AWS as a scalable cloud machine to use for whatever your purposes are, *so long as you do not break terms of service (oops)*. These allow you to set up a virtual machine, such as a ubuntu instance, and have it running 24/7 for free. Given that this is publicly internet accessible, it is important to secure your instance. They are pretty easy to lock down using SSH public key authentication, but that doesn't mean you won't have a whole bunch of logs to parse through with failed authentication attempts, so it is a great example for getting some experience under the hood looking at events.  


<h2>Cats, Tails, Pipes, and Grep</h2>  
The easiest way to get started peeking at events within your system is pretty simple. You are probably familiar with ```cat```, the command short for concatenate. Within the context of the command line interface, this will take a file and spit out the text. So, what is the simplest way to regurgitate login attempts to our machine? Knowing cat, all we really need to know now is where our log files are kept. Within the file system root folder, aka as high as you are going to go, we have ```/var/```. ```/var/``` is for variable data files, so you will tend to find things like logs, databases, email inboxes, and more. Navigating to ```/var/log/```, we can see that we have a bunch of different log files. Syslogs, logs for our package manager, and more. What we are looking for is ```auth.log```:  

```  

ubuntu@nimbus:/var/log$ cd /
ubuntu@nimbus:/$ ls
bin   dev  home  lib32  libx32      media  opt   root  sbin  srv  tmp  var
boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  sys  usr
ubuntu@nimbus:/$ cd var/
ubuntu@nimbus:/var$ ls
backups  cache  crash  lib  local  lock  log  mail  opt  run  snap  spool  tmp  www
ubuntu@nimbus:/var$ cd log/
ubuntu@nimbus:/var/log$ ls
alternatives.log       auth.log.3.gz          dmesg.1.gz      fail2ban.log.2.gz  landscape    syslog.6.gz
alternatives.log.1     auth.log.4.gz          dmesg.2.gz      fail2ban.log.3.gz  lastlog      syslog.7.gz
alternatives.log.2.gz  btmp                   dmesg.3.gz      fail2ban.log.4.gz  private      ubuntu-advantage.log
amazon                 btmp.1                 dmesg.4.gz      journal            syslog       ubuntu-advantage.log.1
apache2                cloud-init-output.log  dpkg.log        kern.log           syslog.1     unattended-upgrades
apt                    cloud-init.log         dpkg.log.1      kern.log.1         syslog.2.gz  wtmp
auth.log               dist-upgrade           dpkg.log.2.gz   kern.log.2.gz      syslog.3.gz
auth.log.1             dmesg                  fail2ban.log    kern.log.3.gz      syslog.4.gz
auth.log.2.gz          dmesg.0                fail2ban.log.1  kern.log.4.gz      syslog.5.gz  

```

So how do we look at any of them? The easiest way is to simply run ```cat /var/log/logname```. This returns a boatload of results, however, for our EC2 instance. Therefore, I decided to simply display one hour's worth of auth.log entries. This can easily be done using something along the lines of ```cat /var/log/auth.log | grep "May 31 07"```. What I am doing here is taking what would be outputted, ```cat /var/log/auth.log```, and *piping* it (``` | ```) into another command; i.e. sending output to be further processed before return to me. What the ```grep``` function is doing is only returning lines with the pattern "May 31 07" present, presenting a much more manageable list:

```  

ubuntu@nimbus:~$ cat /var/log/auth.log | grep "May 31 07"
May 31 07:07:22 nimbus sshd[52959]: Invalid user abc from 104.248.172.241 port 60580
May 31 07:07:22 nimbus sshd[52959]: Received disconnect from 104.248.172.241 port 60580:11: Bye Bye [preauth]
May 31 07:07:22 nimbus sshd[52959]: Disconnected from invalid user abc 104.248.172.241 port 60580 [preauth]
May 31 07:11:46 nimbus sshd[52966]: Received disconnect from 185.18.126.114 port 33872:11: Bye Bye [preauth]
May 31 07:11:46 nimbus sshd[52966]: Disconnected from authenticating user root 185.18.126.114 port 33872 [preauth]
May 31 07:17:01 nimbus CRON[52974]: pam_unix(cron:session): session opened for user root by (uid=0)
May 31 07:17:01 nimbus CRON[52974]: pam_unix(cron:session): session closed for user root
May 31 07:19:05 nimbus sshd[52981]: Invalid user guest-olyoaf from 49.232.55.26 port 47378
May 31 07:19:05 nimbus sshd[52981]: Received disconnect from 49.232.55.26 port 47378:11: Bye Bye [preauth]
May 31 07:19:05 nimbus sshd[52981]: Disconnected from invalid user guest-olyoaf 49.232.55.26 port 47378 [preauth]
May 31 07:24:54 nimbus sshd[52993]: Invalid user virtualbox from 42.192.3.75 port 50440
May 31 07:24:54 nimbus sshd[52993]: Received disconnect from 42.192.3.75 port 50440:11: Bye Bye [preauth]
May 31 07:24:54 nimbus sshd[52993]: Disconnected from invalid user virtualbox 42.192.3.75 port 50440 [preauth]
May 31 07:25:06 nimbus sshd[52991]: Connection closed by 162.142.125.54 port 56598 [preauth]
May 31 07:25:34 nimbus sshd[52995]: Invalid user ynwang from 49.232.198.139 port 59654
May 31 07:25:34 nimbus sshd[52995]: Received disconnect from 49.232.198.139 port 59654:11: Bye Bye [preauth]
May 31 07:25:34 nimbus sshd[52995]: Disconnected from invalid user ynwang 49.232.198.139 port 59654 [preauth]
May 31 07:26:05 nimbus sshd[52998]: Invalid user serverpilot from 81.71.32.72 port 47468
May 31 07:26:05 nimbus sshd[52998]: Received disconnect from 81.71.32.72 port 47468:11: Bye Bye [preauth]
May 31 07:26:05 nimbus sshd[52998]: Disconnected from invalid user serverpilot 81.71.32.72 port 47468 [preauth]
May 31 07:30:10 nimbus sshd[53005]: Invalid user azure from 62.234.141.86 port 50614
May 31 07:30:11 nimbus sshd[53005]: Received disconnect from 62.234.141.86 port 50614:11: Bye Bye [preauth]
May 31 07:30:11 nimbus sshd[53005]: Disconnected from invalid user azure 62.234.141.86 port 50614 [preauth]
May 31 07:30:55 nimbus sshd[53008]: Invalid user test2 from 142.93.251.1 port 36820
May 31 07:30:55 nimbus sshd[53008]: Received disconnect from 142.93.251.1 port 36820:11: Bye Bye [preauth]
May 31 07:30:55 nimbus sshd[53008]: Disconnected from invalid user test2 142.93.251.1 port 36820 [preauth]
May 31 07:35:40 nimbus sshd[53017]: Invalid user matt from 121.4.94.81 port 46606
May 31 07:35:40 nimbus sshd[53017]: Received disconnect from 121.4.94.81 port 46606:11: Bye Bye [preauth]
May 31 07:35:40 nimbus sshd[53017]: Disconnected from invalid user matt 121.4.94.81 port 46606 [preauth]
May 31 07:36:07 nimbus sshd[53020]: Invalid user minecraft from 62.234.73.230 port 36558
May 31 07:36:07 nimbus sshd[53020]: Received disconnect from 62.234.73.230 port 36558:11: Bye Bye [preauth]
May 31 07:36:07 nimbus sshd[53020]: Disconnected from invalid user minecraft 62.234.73.230 port 36558 [preauth]
May 31 07:36:29 nimbus sshd[53022]: error: kex_exchange_identification: Connection closed by remote host
May 31 07:37:31 nimbus sshd[53024]: Invalid user gituser from 114.35.182.104 port 48951
May 31 07:37:31 nimbus sshd[53024]: Received disconnect from 114.35.182.104 port 48951:11: Bye Bye [preauth]
May 31 07:37:31 nimbus sshd[53024]: Disconnected from invalid user gituser 114.35.182.104 port 48951 [preauth]
May 31 07:39:07 nimbus sshd[53029]: Invalid user oracle from 187.32.120.215 port 54110
May 31 07:39:08 nimbus sshd[53029]: Received disconnect from 187.32.120.215 port 54110:11: Bye Bye [preauth]
May 31 07:39:08 nimbus sshd[53029]: Disconnected from invalid user oracle 187.32.120.215 port 54110 [preauth]
May 31 07:40:50 nimbus sshd[53033]: Invalid user junkai_liu from 210.72.91.6 port 10287
May 31 07:40:50 nimbus sshd[53033]: Received disconnect from 210.72.91.6 port 10287:11: Bye Bye [preauth]
May 31 07:40:50 nimbus sshd[53033]: Disconnected from invalid user junkai_liu 210.72.91.6 port 10287 [preauth]
May 31 07:46:29 nimbus sshd[53042]: Unable to negotiate with 45.133.1.115 port 34484: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1 [preauth]
May 31 07:59:28 nimbus sshd[53061]: Invalid user pi from 177.128.2.215 port 57008
May 31 07:59:28 nimbus sshd[53062]: Invalid user pi from 177.128.2.215 port 57016
May 31 07:59:28 nimbus sshd[53061]: Connection closed by invalid user pi 177.128.2.215 port 57008 [preauth]
May 31 07:59:28 nimbus sshd[53062]: Connection closed by invalid user pi 177.128.2.215 port 57016 [preauth]  

```  

So what useful information do we see here? Well, first of all we can concretely tell that it is probably a good thing that we used an SSH key authentication method, because we have about 15 different machines attempting to access ours. We can also see that some of them are attempting to access specific users associated with services, such as "azure", "oracle", "virtualbox", and more. If this is only over the course of an hour, imagine how many attempts occur over weeks, months, or years?  

Let's also take a look at our most recent logs. This can be done using ```tail```, a command which outputs the last 10 lines of a file, similar to cat. For log purposes, sometimes it is nice to run the command as ```tails -f /what/to/output```, which will continue to display new entries as they are appended. Here's what I get for my most recent logs:  

```  

ubuntu@nimbus:/var/log$ tail -f /var/log/auth.log
May 31 09:50:29 nimbus sshd[1634]: Unable to negotiate with 188.166.93.7 port 49040: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1 [preauth]
May 31 09:51:37 nimbus sshd[1637]: Unable to negotiate with 188.166.93.7 port 57898: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1 [preauth]
May 31 09:52:43 nimbus sshd[1641]: Unable to negotiate with 188.166.93.7 port 38522: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1 [preauth]
May 31 09:53:47 nimbus sshd[1644]: Unable to negotiate with 188.166.93.7 port 47194: no matching key exchange method found. Their offer: diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1 [preauth]
May 31 09:54:06 nimbus sshd[1647]: error: kex_exchange_identification: read: Connection reset by peer
May 31 09:54:16 nimbus sshd[1648]: Connection reset by 81.161.63.100 port 45458 [preauth]
May 31 09:54:16 nimbus sshd[1649]: Connection reset by 81.161.63.100 port 45464 [preauth]
May 31 10:00:14 nimbus sshd[1660]: Invalid user virtualbox from 157.92.13.155 port 46920
May 31 10:00:14 nimbus sshd[1660]: Received disconnect from 157.92.13.155 port 46920:11: Bye Bye [preauth]
May 31 10:00:14 nimbus sshd[1660]: Disconnected from invalid user virtualbox 157.92.13.155 port 46920 [preauth]  

```  


The login attempts are constant! This is why proper authentication is so profoundly important. In reality, due to having previously installed ```fail2ban```, we have a daemon running to ban IPs showing malicious signs already. Fail2Ban runs by monitoring our logs, similar to how we might do so, and creating a ban list to alter our firewall table rules via certain criteria. If you think about how you might monitor these logs and alter your firewall rules, you would probably do so in a similar way. Eg, machine A repeatedly fails login attempts 4 times in 10 minues, machine A's IP is banned for x amount of time explicitely within the firewall rules (iptables). This means we aren't even seeing as many login attempts as we typically would. Let's take a look at our fail2ban logs. As we already saw navigating the filesystem earlier, we have ```/var/log/fail2ban.log``` to take a peek at:  

```  

ubuntu@nimbus:~$ cat /var/log/fail2ban.log | grep "Ban"
2021-05-30 00:51:39,363 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-30 02:09:21,663 fail2ban.actions        [448]: NOTICE  [sshd] Ban 109.116.136.119
2021-05-30 04:55:31,223 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-30 09:20:14,982 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-30 13:39:00,152 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-30 17:49:44,775 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-30 18:01:34,989 fail2ban.actions        [448]: NOTICE  [sshd] Ban 106.102.0.44
2021-05-30 18:17:47,557 fail2ban.actions        [448]: NOTICE  [sshd] Ban 218.102.252.139
2021-05-30 22:07:14,470 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-31 01:17:35,441 fail2ban.actions        [448]: NOTICE  [sshd] Ban 87.241.1.186
2021-05-31 02:02:09,744 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197
2021-05-31 02:52:25,944 fail2ban.actions        [448]: NOTICE  [sshd] Ban 109.64.19.179
2021-05-31 03:02:47,039 fail2ban.actions        [448]: NOTICE  [sshd] Ban 109.64.19.179
2021-05-31 03:13:07,921 fail2ban.actions        [448]: NOTICE  [sshd] Ban 109.64.19.179
2021-05-31 06:26:47,706 fail2ban.actions        [448]: NOTICE  [sshd] Ban 209.141.36.197  

```  

Looking into this, we have a few repeat offenders:  
- 106.102.0.44  
- 109.116.136.119  
- 109.64.19.179  
- 209.141.36.197  
- 218.102.252.139  
- 87.241.1.186  

And that just leads to further fun via shodan.io and more :)   
