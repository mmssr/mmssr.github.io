---
layout: post
title: "Linux Command Line Cheatsheet"
date: 2021-02-04
---

<h2>/* File/Basic Navigation */</h2>  
cd - change directory  
cd ~ - home folder  
cd .. - go up one folder  
mkdir - make directory  
rmdir - remove directory (-r for recursive)  
rm - remove file  
mv [filename] [directory] - move file to directory (eg mv test.txt Downloads/)  
pwd - see present working directory  
ls - list subdirectories (-la to include hidden)  
locate [keyword] - locate a file (eg locate bash)  
updatedb - update filesystem database  
man [command] - see options and instructions for command (eg man ls)  
[command] -\-help - usually a simpler version of man  
cat [filename] - print contents of a file to terminal  
grep [pattern] - capture lines with pattern, (eg ping google.com -c 1 | grep "64 bytes")  
cut - allows you to cut based upon delimiter (-d "[delim]) and sections (-f [which section]), (eg ping google.com -c 1 | grep "64 bytes" | cut -d " " -f 4)  
tr - allows you to trim/remove chars from text based upon char (-d [char you want to remove]), (eg echo te:st | tr -d ":")  


<h2>/* Basic User Stuff */</h2>  
psswd - change password for current user  
useradd - creates new user, (eg adduser John)  
usermod - modify an existing user  
userdel - remove a user & related files  
groupadd - add a group  
groupmod - modify a group  
groupdel - remove a group  
su - switch user, (eg su John)  
sudo - super user perm  
visudo - opens /etc/sudoers file for edit, exclusively use this to manage /etc/sudoers  
cat /etc/passwd - see all users  


<h2>/* File permissions, editing, viewing, and creating */</h2>  
<h3>/* chmod and file properties */</h3>
chmod - change mode, edits access (eg chmod 0777 hello.txt (gives all file permissions. always user 4 digits.), chmod +x (makes executable))  
```[file perms] [hard link count] [owner name] [owner group] [file size (bytes)] [last mod time] [name]```  
![chmod](/assets/chmod.png)  
First char, d or -, stands for directory (d) versus file (-) The next 9 characters are rwx, rwx, rwx for the owner, owner group, and all other users. The letters stand for read (r), write (w), or execute (x). For the .bash_history for instance, we see it is a file, the owner can read or write to it, the owner group can read it, and all others can read it. The file also belongs to root, and is within the root owner group.  
<h3>/* editing, viewing, and creating files */</h3>
echo "text" > filename.txt - creates a file by the name of filename.txt with the contents "text"  
echo "more text" >> filename.txt - appends "more text" to filename.txt  
cat filename.txt - prints filename.txt contents to terminal  
touch newfile.txt - creates a new file titled newfile.txt  
nano newfile.txt - opens (or creates) newfile.txt in nano terminal text editor  
gedit newfile.txt  - opens newfile.txt in gedit graphical text editor  


<h2>/* Network Commands */</h2>  
ifconfig - shows your wired interface info  
iwconfig - shows your wireless inferface info  
ping - ping address, (eg ping google.com -c 3 (tries to ping google.com 3x))  
arp - shows you ap associated with mac (usually use arp -a)  
netstat - shows active connections on machine (usually use netstat -ano)  
route - shows your routing table. essentially shows your local traffic exit for potential pivot if there are multiple routes.  


<h2>/* Starting and stopping services - web host example*/</h2>  
<h3>/* ideal - systemctl gives greater control but not always available */</h3>  
systemctl start apache2 - start apache webserver  
systemctl stop apach2 - stop apache webserver  
<h3>/* backup */</h3>  
service apache2 start - start apache webserver  
service apache2 stop - stop apache webserver  
systemctl enable [service] - enable service to begin at startup, (eg systemctl enable postgresql)  
systemctl disable [service] - disable service to begin at startup, (eg systemctl disable postgresql)  
systemctl list-units -\-type=service - lists all services  
ps -aux | grep [service name] - check for running service  
journalctl -u [servicename].service --no-pager - view the logs associated with a service  


<h2>/* Process kills */</h2>  
kill -l - view all kill signals  
kill -[signal number] [process id] - kill process with specified signal  
kill -1 [process id] - signal that process terminal has closed  
kill -2 [process id] - signal to interrupt a process, or ctl+c in the controlling terminal session  
kill -3 [process id] - signal sent when ctl+d is entered in terminal controlling session  
kill -9 [process id] - signal sent to immediately kill process, no catch/ignore possible  
kill -20 [process id] - signal sent to suspend a process, can be used later; also sent by ctl+z in controlling terminal session.  


<h2>/* Process foreground/background */</h2>  
todo  


<h2>/* Starting python webserver or ftp server */</h2>  
python -m SimpleHTTPServer [port number] - starts a python webserver in your current subdirectory at given port  
python -m pyftpdlib -p [port number] - starts a python ftp server in your current subdirectory at given port  


<h2>/* Installing and updating tools via APT */</h2>  
apt update - updates the list of available packages and their versions  
apt upgrade - installs newer versions of packages you have  
apt update && apt upgrade - both  
apt install [package name] - installs a specific package using apt  
apt list -\-installed - shows all installed packages  
cat /etc/apt/sources.list -  show package repository information  
apt-cache search [package name] - basically runs grep on local package descriptions  
apt-cahe show [package name] - shows additional details about a given package  


<h2>/* Installing and updating tools on github via GIT */</h2>  
1. Copy the "clone or download link" on github  
2. Navigate to /opt/  
3. git clone [link] (or do both as mkdir /opt/[name] && git clone [link] /opt/[name])  
4. change directory to the dir you just created  
5. follow any additional instructions given, such as "pip install ." or "make && make test"  


<h2>/* Installing via DPKG */</h2>  
1. wget [link to .deb package] - download the tool directly  
2. dpkg -i [.deb package filename] -  installs tool, generally immediately ready to use  
