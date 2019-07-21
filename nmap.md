# Nmap - usefull scans

## Top 100
```
nmap  --top-ports 100 --open 10.11.1.*
```

## Basic
```
nmap  -sC -sV -O 10.11.1.*
```

## All ports
```
nmap  -sC -sV -O -p- 10.11.1.*
```

## UDP
```
nmap  -sU 10.11.1.*
```

## Enum
- ping scan 
```
nmap -v -sn 10.11.1.1-254 -oG ping-result.txt 
grep Up ping-result.txt | cut -d ' ' -f2 > ip_list.txt
```

- use list ip scanned
```
nmap -p 80,443 -O -iL ip_list.txt -oG web.txt
```




