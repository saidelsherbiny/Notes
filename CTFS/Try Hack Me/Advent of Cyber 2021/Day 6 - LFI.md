# Day 6 - Web - LFI

## What is a Local File Inclusion (LFI) vulnerability?

- Web app vulnerability that allows an attacker to include and read files localy on a web server. 
- It occurs due to a developers lack of security awareness. 
- The lack of input validation, where the users inputs are not sanitized or validated, is the main issue of this vulnerability.
- In PHP:
	- `include`
	- `require`
	- `include_once`
	- `require_once`

## Risks
- Information disclosure
- Leaking of sensitive data
- Possible RCE when chained

## Identifying and testing
- HTTP GET or POST parameters that pass an argument to the web application to perform a specific operation

![](https://i.imgur.com/yVemcMz.png)

## Exploiting LFI

### PHP filter

- Uses in LFI to read actual page content
- Not possible to read page content normally because PHP files get executed and never show the existing code.
- Examples:
![](https://i.imgur.com/r1Q2ml8.png)
![](https://i.imgur.com/1Fhu4ad.png)

### LFI to RCE via LOG files (LOG poisoning attack)
-  The attacker needs to include a malicious payload into services log files such as Apache, SSH, etc. Then, the LFI vulnerability is used to request the page that includes the malicious payload.
-  The User-Agent is an HTTP header that includes the user's browser information to let servers identify the type of operating system, vendor, and version. 
	-  The User-Agent is one of the HTTP headers that the user can control. 
	- Therefore, in order to get the RCE, you need to include PHP code into User-Agent and send a request to the log file using the LFI to execute in the browser.
![](https://i.imgur.com/iZaSNCL.png)

### LFI to RCE via PHP sessions
- This technique requires enumeration to read the PHP configuration file first, and then we know where the PHP sessions files are. 
- Then, we include a PHP code into the session and finally call the file via LFI.
