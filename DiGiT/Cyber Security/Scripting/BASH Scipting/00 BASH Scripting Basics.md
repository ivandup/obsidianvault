### While Loop:

*Specify the length:*
*-le => length*

``````
n=1
	while [ $n -le 5 ] 
do
	echo "Testing $n times!"
	n=$(( n+1 ))
done
``````

##### *Example output:*
*Testing 1 times!*
*Testing 2 times!*
*Testing 3 times!*
*Testing 4 times!*
*Testing 5 times!*

----
### For Loop:

*counter variable 1 (counter = 5 is the max value*
*counter variable 2 (counter > 0 is the min value*
*counter variable 3 (counter-- subtracts a value*

```
for (( counter =5; counter>0; counter--))
	do
		echo -n "$counter"
	done
printf "\n"
```

````
##### Example output:
54321
````

---
### User Input:

```
#!/bin/bash
echo "Enter Name:"
read name
echo "Welcome "$name" to Cyber World"
```

### IF statements:

```
#!/bin/bash
echo "Enter Username: "
read username
echo "Enter Password: "
read password
if [[( $username == "admin" && $password == "secret" ) ]];
then
	echo "Valid user"
else
	echo "Invalid user"
fi
```

Reading arguments:
------------------
```
#!/bin/bash
echo "Total arguments: $#"
```

#### Read the number of arguments passed
```
echo "1st arg =$1"
echo "2nd Argument = $2"
```

Example output:
./test.sh training script
Total arguments: 2
1st arg =training
2nd Argument = script

#### Using other tools in bash script:
Want to know if a user types any inputs
```
#!/bin/bash
if [${#} -eq 0 ]
then
echo "You need to enter a valid SSH server"
exit 1
else
echo "The DNS beig used by the server is: "

```
Now, run the command
```
ssh digit@$1 "cat /etc/resolve.conf" | grep nameserver
fi
```



