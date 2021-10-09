# Pickle Rick

- Ports: 80, 22, 

- robots.txt > Wubbalubbadubdub
- Homepage source > Username: R1ckRul3s

- gobuster > /assets > just some pictures
- GOBUSTER! You fickel mistress. You missed the `/index.php`. Had to find it from the nmap vuln scan

- user: R1ckRul3s, pass: R1ckRul3s

- Use command panel on login to get first flag

## Flag1: "mr. meeseek hair"


- Hint (clue.txt) in www dir says to searcch the filesystem for the other ingredients

`find -type f -iname "*Ingred*" 2> /dev/null` we find `/home/rick/second ingredient` text file

## Flag2: "1 jerry tear"

If you check `sudo -l` apparently we already have sudo
Find 3rd flag in `/root`

## Flag3: "fleeb juice"