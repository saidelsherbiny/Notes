## DNS enumeration
nslookup
`export TARGET="google.com"
`nslookup -query=ANY $TARGET
dig
whois

`Resource Record`A domain name, usually a fully qualified domain name, is the first part of a Resource Record. If you don't use a fully qualified domain name, the zone's name where the record is located will be appended to the end of the name.
`TTL`In seconds, the Time-To-Live (`TTL`) defaults to the minimum value specified in the SOA record.
`Record Class`Internet, Hesiod, or Chaos
`Start Of Authority` (`SOA`)It should be first in a zone file because it indicates the start of a zone. Each zone can only have one `SOA` record, and additionally, it contains the zone's values, such as a serial number and multiple expiration timeouts.
`Name Servers` (`NS`)The distributed database is bound together by `NS` Records. They are in charge of a zone's authoritative name server and the authority for a child zone to a name server.
`IPv4 Addresses` (`A`)The A record is only a mapping between a hostname and an IP address. 'Forward' zones are those with `A` records.
`Pointer` (`PTR`)The PTR record is a mapping between an IP address and a hostname. 'Reverse' zones are those that have `PTR` records.
`Canonical Name` (`CNAME`)An alias hostname is mapped to an `A` record hostname using the `CNAME` record.
`Mail Exchange` (`MX`)The `MX` record identifies a host that will accept emails for a specific host. A priority value has been assigned to the specified host. Multiple MX records can exist on the same host, and a prioritized list is made consisting of the records for a specific host.

## Subdomain enumeration
Automation tools:
- TheHarvester:
	This tool can use websites such as the following to find:`emails`, `names`, `subdomains`, `IP addresses`, and `URLs`
	
	-  Baidu	Baidu search engine.
	-  Bufferoverun	Uses data from Rapid7's Project Sonar 
	-  Crtsh	Comodo Certificate search.
	- Hackertarget	Online vulnerability scanners and network intelligence to help organizations.
	- Otx	AlienVault Open Threat Exchange 
	- Rapiddns	DNS query tool, which makes querying subdomains or sites using the same IP easy.
	- Sublist3r	Fast subdomains enumeration tool for penetration testers
	- Threatcrowd	Open source threat intelligence.
	- Threatminer	Data mining for threat intelligence.
	- Trello	Search Trello boards (Uses Google search)
	- Urlscan	A sandbox for the web that is a URL and website scanner.
	- Vhost	Bing virtual hosts search.
	- Virustotal	Domain search.
	- Zoomeye	A Chinese version of Shodan.

Example:

```shell-session
cat sources.txt

baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

Run the tool
```shell-session
export TARGET="facebook.com"
cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}_${TARGET}";done
```

Export all the subdomains:
```shell-session
cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt"
```

Merge all reconnaissance files:
```shell-session
cat facebook.com_*.txt | sort -u > facebook.com_subdomains_passive.txt
cat facebook.com_subdomains_passive.txt | wc -l
```


## Passive Infrastructure Identification

- Netcraft: https://sitereport.netcraft.com
- Wayback Machine:
	we can install via:
```shell-session
go install github.com/tomnomnom/waybackurls@latest
```
example:
 ```shell-session
waybackurls -dates https://facebook.com > waybackurls.txt
cat waybackurls.txt
```

#### HTTP headers
exmaple:
```shell-session
curl -I "http://${TARGET}"
```

- X-Powered-By header: This header can tell us what the web app is using. We can see values like PHP, ASP.NET, JSP, etc.  
- Cookies: Cookies are another attractive value to look at as each technology by default has its cookies. Some of the default cookie values are:
	- .NET: `ASPSESSIONID<RANDOM>=<COOKIE_VALUE>`
	- PHP: `PHPSESSID=<COOKIE_VALUE>`
	- JAVA: `JSESSION=<COOKIE_VALUE>`
Tools:
- whatweb  -find info about infrastructure including CMS
	example:
	```shell-session
	whatweb -a3 https://www.facebook.com -v
	```
- Wappalyzer
- WafW00f
	Installation:
```shell-session
sudo apt install wafw00f -y

```

Example:
```shell-session
wafw00f -v https://www.tesla.com
```

- Aquatone:
	Installation
```shell-session
sudo apt install golang chromium-driver
go get github.com/michenriksen/aquatone
export PATH="$PATH":"$HOME/go/bin"
```

Example:
```shell-session
cat facebook_aquatone.txt | aquatone -out ./aquatone -screenshot-timeout 1000
```

## Active domain Enumartion

finding FQDN