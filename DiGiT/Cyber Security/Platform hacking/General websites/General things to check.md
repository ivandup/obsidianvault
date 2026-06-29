# General observation
See if there are any user details listed on the website somewhere. 
1.) You can perhaps try to brute force the password for these accounts.
2.) Do a Google search for the username and see if you can find anything. perhaps there are github pages with configs, try using the passwords in the config to crack the password


# Check of login pages
Try default details such as:
```
admin/admin
```

# Try simple SQL injection:
Try the folloing for username and password
```
u/n: admin ' OR 1=1 --
pwd: something
```

View the page source code for anything useful.

# Check the robots.txt file
Go to url/robots.txt, example:
```
http://10.50.10.68/robots.txt
```
Here there should be listed some directories which should be excluded for search engines. try these directories and see if there is anything interisting.

# Nikto
Run a nikto scan for enumeration
```
nikto -h http://10.50.10.68
```

Look for anything outdated and do a searchsploit on it
example:
```
searchsploit apache 2.2.25
```

Check for any possible config files.

# GoBuster
Use GoBuster to see if you can find any directory listings:
```
gobuster dir -u http://10.50.10.68 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

OR 

```
gobuster dir -u http://10.50.10.68 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.html,.txt,.sh,.js,.bak

# -x Options:
.php,.html,.txt,.bak
```

# Similar Images
If a website has similar images where it might be ususpiciousm try downloading them and check if there are any file diferences between them. If there are, you might want to look into stegnogorophy.
