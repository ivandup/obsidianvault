#### Getting details from files & modifying them:

##### Convert uppercase to lower case
```bash
cat first-names.txt | tr'[:upper]' '[:lower]'
```

#### Only display the first 20 lines
```bash
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20
```

#### Normilise the file to a unix format:
```bash
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix
```

#### add  prefix (www.) to the beginning words and add a ",com" at the end of each word
#### !!! If it's not a unix format, the .com will appear in the front of the word, not the back.
```
cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'
```


##### Adding the for loop in one line (Notice the tindel single quote, not the normal single quote):
```bash
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done
```

#### for a cleaner output, example, only output files that contain "has address":

```bash
for i in `cat first-names.txt | tr'[:upper]' '[:lower]' | head -n 20 | dos2unix | awk '$0="www."$s0".com"'`; do host $1;done | awk '/has address/ { print $4 }'
```

# Unique words
If a files has multiple fields which might be duplicates, you can remove it by runnung:
```bash
cat <filesName>.txt | uniq > fileNameNew.txt
```

linux cli. Using cat to remove duplicate words in a word list

**If order doesn't matter (sorted output):**
```bash
sort wordlist.txt | uniq > cleaned.txt
```
`uniq` only collapses _adjacent_ duplicates, which is why `sort` comes first. You can also do it in one tool:
```bash
sort -u wordlist.txt > cleaned.txt
```

**If you want to preserve the original order** (keep first occurrence of each word):
```bash
awk '!seen[$0]++' wordlist.txt > cleaned.txt
```

This is the one people usually want for wordlists, since it doesn't reorder anything.

**A few useful variants:**
Case-insensitive dedupe (treats `Word` and `word` as the same):
```bash
sort -uf wordlist.txt > cleaned.txt
```

If your "words" are one per line, all of the above work directly. If instead you have multiple words per line and want to dedupe the individual words, first split them onto separate lines:
```bash
tr ' ' '\n' < wordlist.txt | awk '!seen[$0]++' > cleaned.txt
```

Since you mentioned `cat`, note that piping through it (`cat wordlist.txt | sort -u`) works but is unnecessary here—the tools read files directly. The one time `cat` genuinely helps is combining several lists before deduping:
```bash
cat list1.txt list2.txt list3.txt | awk '!seen[$0]++' > merged.txt
```

What does your wordlist look like—one word per line, or multiple words per line?