# Included - Tier 3 

## Enumeration

- UDP scan `nmap -sU`
- Discover tftp, upload php reverse shell

## Foothold
- We are www-data
- Discover .htpasswd file which has credentials mike:Sheffield19

## Priv-esc
- Discover that mike is part of the `lxd` group
- Potential privesc with that:

https://book.hacktricks.xyz/linux-unix/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation

https://github.com/lxc/distrobuilder