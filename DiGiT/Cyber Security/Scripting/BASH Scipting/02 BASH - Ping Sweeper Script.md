Ping out:
![[Pasted image 20260419040333.png]]


ping-loop.sh
```
#/bin/bash
fip ip in$(seq 200 210); do
ping -c 1 192.168.10.$ip |grep "bytes from |cut -d" " -f4 |cut -d":" -f1 &
done
```
|grep "bytes from" => grep for teh results
|cut -d" " -f4 => cut with deliliter of space and display the 4th field (The IP address)
|cut -d":" -f1 => cut again with delimiter of ":" and display the first field
& => Let the command run in background to speed up the script