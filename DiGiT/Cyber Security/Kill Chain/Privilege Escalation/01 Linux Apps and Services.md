Checking all the process running:
```
ps aux
```
x => user, in this case any

View processes for root, replace x with username (e.g.: root)
```
ps auroot
```

```
ps ef
```
e => every process
f =>

Top
```
top
htop
```

Services & Ports running
```
cat /etc/services # which service run on which port
netstat -an
netstat -antp
cat /etc/hosts
ls -alh /usr/bin # Contains most files/apps
ls -alh /sbin # Executable files/apps - mostly admin tools
dpkg -l # List of installed packages
```

### misconfigured plugins
```
cat /etc/syslog 
cat /etc/apache2/apache2.conf
```

### Cron
If any cronjob run, you can perhaps modify the file to run something you want.
```
craontab -l
ls -lah /etc/cron
ls -lah /etc/crontab
```

Search for files containing pass
```
grep -iRL "pass" /
```
i => case not sensitive
R => recursive
L => list file names, not context.
