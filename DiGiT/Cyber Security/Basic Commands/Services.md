Enable service at startup:
```
systemctl enable <serviceName>
```

or 

```
update-rc.d <serviceName> enable
```

Example:
```
update-rc.d apache2 enable
```

On Kali you can also use the following commands to easily enable/disable services:
```
rcconf
```

OR

```
sysv-rc-conf
```