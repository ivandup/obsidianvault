Find out charset on generate a password list with only numbers (Example PIN NUmber):
```
crunch 4 4 -f /usr/share/crunch/charset.lst numeric
```

Generate password list with spesific letter and numbers.
Example, generate a password list with of 4 characters long using the letters "abc" and the number "1"
```
crunch 4 4 abc1
```

Sample example output:
```
11ba
11bb
11bc
11b1
11ca
11cb
11cc
11c1
111a
111b
111c
1111
```

Another example using 6 characters and output to a file:
```
crunch 6 6 0123456789abcdef -o 6chars.txt
```

 