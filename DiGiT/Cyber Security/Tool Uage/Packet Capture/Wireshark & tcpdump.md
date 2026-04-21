### TCPdump
Capture packets with tcpdump
```
tcpdump -x -X -i eth0 'port 1234'
```
-i => Interface (e.g.: eth0)
'port 1234' => Port to filter (e.g.: port 1234')

### Wireshark

Wireshark UI
Select the interface and start capture.

#### Filters:
Search or a string (e.g.: thisisatest)
![[Pasted image 20260404075940.png]]

### WireShark terminal (tshark):
tshark -V -i eth0 'tcp port 80'
-V => See the payload
-i => Interface
'tcp port 80' => filter options (e.g.: TCP port 80). Can also use grep, awk or sed or other commands.

tshark -V -i eth0 'tcp port 80' | grep "For "

