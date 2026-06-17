
fgdump, dumps the passwords NTLM hashes of a windows device.
fgDump located on Kali:
```
/usr/share/windows-binaries/fgdump.exe
```

Copy this to the target PC and run it as is. It will dumps all the files to the current directory by default.

Once you have the hashes, you can copy it to your Kali PC and crack it.

Copy the NTLM Hashes into a text files, example passntlm.txt
Offline password cracker:
```
john --format=LM --wordlist=<pathToWordList> <pathToNTLMPasswordFile>
```

