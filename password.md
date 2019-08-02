# password cracking

## Crack wordpress password

- `hashcat -a 0 -m 400 hashes /usr/share/wordlist/rockyou.lst`
- `hashcat -a 0 -m 400 hashes /usr/share/wordlist/rockyou.lst -r /usr/share/doc/hashcat/rules/best64.rule`

## Generate specific wordlist with cewl

- `bundle exec cewl.rb https://www.example.com/ -d 1 -m 5 -w exemple.txt`



