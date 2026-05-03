Find out distribution type/version so that you can know which commands to run to escalate privileges or look for exploits.

```
cat /etc/issue
cat /etc/*-release
cat /etc/redhat-release
```

Check Kernal version:
```
cat /proc/version
```

Unix Name:
```
uname -a
uname -mrs
```

```
dmesg | grep Linux
ls /boot/ grep vmlinux
```

Environmental variables:
```
env
cat /etc/profile
```

Shell info
```
cat ~/.bashrc
cat ~/.bashrc_logout
```

Check if printers are connected:
```
lpstat -a
```