# Skynet
open ports: 22, 80, 110, 139, 143, 445

## Enum 
SMB shares: anonymous, milesdyson

Anonymous login gets an `attention.txt` and a `log1.txt` with what appear to be usernames or passwords

Use Burp intruder with user=milesdyson and the log files as a password list
user: milesdyson
password: cyborg007haloterminator

Now we have Miles email access. He was assigned a new smb 
password: )s{A&2Z=F^n_E.B\`


Miles's SMB access gets an `important.txt` which point to Mile's personal page which is the "hidden directory": /45kra24zxs28v3yd

In squirrelmail there is a way to reference the help menu that allows for RFI. The url is:
url: http://10.10.235.211/squirrelmail/src/webmail.php?right_frame=help.php

Had to look at the walkthrough. You apparently need to continue enumerating the website from the hidden directory `45kra24zxs28v3yd`. This reveals an `/administrator` page that's hosting a Cuppa CMS.

Look it up on ExploitDB and there is one RFI [exploit](https://www.exploit-db.com/exploits/25971)

find a php reverse shell and just follow the script to get a www reverse shell

## Privesc
`/var/www/html/45kra24zxs28v3yd/administrator/Configuration.php`
shows user="root" and password="password123" but they don't work for some reason
