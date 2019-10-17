# ELF & pwn

## See sections and headers

- `dumpelf`
- `elfls -p /bin/ps`
- `eu-readelf –section-headers /bin/ps`
- `readelf -S /bin/ps`
- `objdump -h /bin/ps`
- `pwndbg> maintenance info sections`

- show address : `readelf -l /bin/ps`

## Show external libs

- `ldd /bin/ps`

## Struct

```
>>> struct.pack('I',0xAABBCCDD)
'\xdd\xcc\xbb\xaa'
>>> struct.pack('Q',0xAABBCCDD)
'\xdd\xcc\xbb\xaa\x00\x00\x00\x00'
```

- Little indian '<' / big Indian '>' / native '='
```
>>> struct.pack('>I',0xAABBCCDD)
'\xaa\xbb\xcc\xdd'
>>> struct.pack('<I',0xAABBCCDD)
'\xdd\xcc\xbb\xaa'
>>> struct.pack('=I',0xAABBCCDD)
'\xdd\xcc\xbb\xaa'
```

## Bad chars 

```
bad_chars=("\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D\x0E\x0F"
"\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1A\x1B\x1C\x1D\x1E\x1F"
"\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2A\x2B\x2C\x2D\x2E\x2F"
"\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3A\x3B\x3C\x3D\x3E\x3F"
"\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4A\x4B\x4C\x4D\x4E\x4F"
"\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5A\x5B\x5C\x5D\x5E\x5F"
"\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6A\x6B\x6C\x6D\x6E\x6F"
"\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7A\x7B\x7C\x7D\x7E\x7F"
"\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8A\x8B\x8C\x8D\x8E\x8F"
"\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9A\x9B\x9C\x9D\x9E\x9F"
"\xA0\xA1\xA2\xA3\xA4\xA5\xA6\xA7\xA8\xA9\xAA\xAB\xAC\xAD\xAE\xAF"
"\xB0\xB1\xB2\xB3\xB4\xB5\xB6\xB7\xB8\xB9\xBA\xBB\xBC\xBD\xBE\xBF"
"\xC0\xC1\xC2\xC3\xC4\xC5\xC6\xC7\xC8\xC9\xCA\xCB\xCC\xCD\xCE\xCF"
"\xD0\xD1\xD2\xD3\xD4\xD5\xD6\xD7\xD8\xD9\xDA\xDB\xDC\xDD\xDE\xDF"
"\xE0\xE1\xE2\xE3\xE4\xE5\xE6\xE7\xE8\xE9\xEA\xEB\xEC\xED\xEE\xEF"
"\xF0\xF1\xF2\xF3\xF4\xF5\xF6\xF7\xF8\xF9\xFA\xFB\xFC\xFD\xFE\xFF")
```

## Hexadecimal view

- Edit : 
```
xxd -g 4 bin_file | vim -
```

- Save (inside vim)
```
:%!xxd -r > bin_file_modified
```

## Socket connection

```python
import socket
import binascii
import requests
import struct

def recvall(sock):
    BUFF_SIZE = 1024
    data = b''
    while True:
        part = sock.recv(BUFF_SIZE)
        data += part
        if len(part) < BUFF_SIZE:
            break
    return data


def recvuntil(sock, until):
    buf = b''
    while until not in buf:
        newbuf = sock.recv(1)
        if not newbuf: return None
        buf += newbuf
    return buf


def send(sock, data):
    print(data)
    return sock.send(data)

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect((HOST, PORT))
print(recvuntil(s, b'login:'))
send(s, b'login_send')
```

## Convert 

- string <-> int
```
>>> chr(65)
'A'
>>> ord('A')
65
```

- string <-> hex
```
>>> binascii.a2b_hex('41414141')
'AAAA'
>>> binascii.b2a_hex(b'AAAA')
'41414141'
```

- hex <-> int
```
>>> int('0x41414141', 16)
1094795585
>>> hex(65)
'0x41'
>>> hex(1094795585)
'0x41414141'
```

- decode binary -> string
```python
data.decode('utf-8')
data.decode('utf-8','ignore')
data.decode('utf-8', 'backslashreplace')
```

## Format string

- `%x` : show stack value
- `%s` : show value @stack value
- `%u` : show int value
- `%n` : write nb chars already printed @stack value
- `%hn`: write 2 bytes (16bits) instead of 4 bytes (32bits)

- `$` : used to argument index (example : `%5$08x`)

- Example : 
```
AAAA-%08x-%08x-%08x-%08x-%08x
AAAA-08048625-00000000-00000000-0000001e-41414141
AAAA-%5$08x
AAAA-41414141   <- show offset 5 with $
AAAA-%s
AAAA-���   <- value at 0x08048625
AA--BB---%5$hx%6$hx
AA--BB---41414242 <- top 2 bytes at offset 5 and top two bytes at offset 6
```

## GDB

- set value 0x41414141 at address 0xdeadbeef : `set {int}0xdeadbeef = 0x41414141`
- show value at address : `x/1x 0xdeadbeef`
- show string at address : `x/1s 0xdeadbeef`

- pwndbg: 
  - `context` : print all the context values
