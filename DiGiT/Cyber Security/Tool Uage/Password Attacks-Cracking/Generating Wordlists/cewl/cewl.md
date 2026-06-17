Generate a wordlist from a website:
```
cewl -d 2 -m 5 -w /root/Desktop/Files/docswords.txt http://10.211.55.14/bWAPP/ba_pwd_attacks_1.php
```
-d 2 => Goes to the page specified and the next page (one lvel down)
-m 5 => Minumum letter length is 5
-w => Output: Path where the word lists will be written to
http://... => the website it connects to

Example output:
![[Pasted image 20260428062818.png]]

