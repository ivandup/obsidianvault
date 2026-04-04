# Find Command:
#### Find files 2 directories deep:
```
find / -mexdepth -name ".html"
```

#### Find files with permissions, can be exploited with files that can you elevated privileges
Example: Stickybit vulnerability
```
find / -type -f -perm 0664
```


#### Find files access in the last 50 days
```
find / -atime 50
```


#### Find files by size (e.g.: 50M in size)
```
find / -size 50M
```


#### using the find command, how do i find a file which is:
* human-readable
* 1033 bytes in size
* not executable

```
find / -readable -size 1033c ! -executable
```


#### find a file with user bandit7 and group bandit6 with a file size of 33bytes:
```
find / -user bandit7 -group bandit6 -size 33c
```


add "2>/dev/null" to the end of the file so that it doesn't output any permission denied errors:
Example:
```
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

#### Search for a word that is stored in the file data.txt and is the only line of text that occurs only once
```
sort data.txt | uniq -u
```


#### Find text stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
```
strings data.txt | grep '==='
```

# locate command:
Will find any file that you specify, extension, etc.

### updated command
```
updatedb
```

will find all files on the file system and index it

## which Command
```
which -a <fileName>*
```

Find location of a app to show all the places where the app is installed.


### FILES

Check what type of file a file is.
# Example:
```
file /etc/init.d/apache2
```
/etc/init.d/apache2: POSIX shell script, ASCII text executable

#### SERVICES

See what services can run, check the /etc/init.d directory
```
ls /etc/init.d
```

```
update-rc.s <serviceName> <enable|disable|start|stop>
```



