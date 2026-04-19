
# Using multiple commands

Get a list of their sub-domain by downloading the index file:
```
wget www.cisco.com
```

If you view the HTML file, there will be link to some sub-domains in the hyperlink section

Grep out all lines with href:
```
cat index.html |grep "href="
```

To extract the domain names without the http://
```
cat index.html |grep "href=" |cut -d"/" -f3 |more
```
-d"/" => delimiter: is what to cut
-f3 => Print the 3rd field

Example output:
![[Pasted image 20260418051908.png]]

You'll notice some entries which are useless, example, login?refere=, to clean this out, run:
Filter out records which does not contain cisco.com
```
cat index.html |grep "href=" |cut -d"/" -f3 |grep "cisco\.com"
```
Example output:
![[Pasted image 20260418052132.png]]

As this looks better, there are still some useless stuff, example, ">Learning network<
Clean the entries
```
cat index.html |grep "href=" |cut -d"/" -f3 |grep "cisco\.com" |cut -d'"' -f1
```
Output should be cleaned up not, but there might be duplicates, remove them by using the sort command:
```
cat index.html |grep "href=" |cut -d"/" -f3 |grep "cisco\.com" |sort -u
```
-u => Unique entries

# Using a single Command with regex:
```
grep -o '[A-Za-z0-9_\.-]*\.*cisco.com' index.html |sort -u
```

Get the IP of the domain name:
```
host <domainName.com>
```

now use the same command, to show only the IP address:
```
host cisco.com |grep "has address" |cut -d" " -f4
```

![[Pasted image 20260418053030.png]]

Writing the bash script to extract all info:
```
vi getDomain.sh
```

## Script:
```
#!/bin/bash

for url in $(cat cisco.txt); do
host $url |grep "has address" |cut -d" " -f4"
done
```

Make executable:
```
chmod 755 getDomain.sh
```

Once you run you should see the IP addresses

## Oneliner bash:
```
for url in$(grep -o '[A-Za-z0-9_\.-]*\.*cisco.com' index.html |sort -u); do host $url|grep "has address"|cut -d" " -f4;done
```
![[Pasted image 20260418053612.png]]