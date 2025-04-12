# baby APT - Forensics

- Analyse .pcap file with wireshark
- Discover that the attacker uploaded a php shell to the web server and is executing commands
- Decode the attackers last command ran on the server
	- `rm  /var/www/html/sites/default/files/.ht.sqlite && echo SFRCezBrX24wd18zdjNyeTBuM19oNHNfdDBfZHIwcF8wZmZfdGgzaXJfbDN0dDNyc180dF90aDNfcDBzdF8wZmYxYzNfNGc0MW59 > /dev/null 2>&1 && ls -al  /var/www/html/sites/default/files`
- Put the echoed text into cyberchef and discover it is base64
- Flag `HTB{0k_n0w_3v3ry0n3_h4s_t0_dr0p_0ff_th3ir_l3tt3rs_4t_th3_p0st_0ff1c3_4g41n}`

