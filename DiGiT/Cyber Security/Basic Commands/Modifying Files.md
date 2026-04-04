#### Getting details from files & modifying them:

##### Convert uppercase to lower case
```
cat first-names.txt | tr'[:upper]' '[:lower]'
```

#### Only display the first 20 lines
```
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20
```


#### Normilise the file to a unix format:
```
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix
```

#### add  prefix (www.) to the beginning words and add a ",com" at the end of each word
#### !!! If it's not a unix format, the .com will appear in the front of the word, not the back.
```
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'
```


##### Adding the for loop in one line (Notice the tindel single quote, not the normal single quote):
`````
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done
`````

#### for a cleaner output, example, only output files that contain "has address":

`````
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done | awk '/has address/ { print $4 }'
`````
