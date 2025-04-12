# Sakura - OSINT

## Tip-off

- Examine exif data in the following image: https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg
- Use http://exif.regex.info/exif.cgi to discover;
	- ![](https://i.imgur.com/udizjGH.png)
	- Analyzing the binary numbers in the image reveals the following message: `A picture is worth 1000 words but metadata is worth far more`

## Reconnaissance

- Seach internet for sakurasnowangelaiko
- Findings:
	- Real name: Aiko Abe
	- Try pimeyes and google reverse image search on linkedin profile, find the following;
	- Twitter handle: https://twitter.com/SakuraLoverAiko
	- Github profile: https://github.com/sakurasnowangelaiko
	- Inspect pgp key on github
	- Use https://cirw.in/gpg-decoder to decode pgp key to uncover email
	- Email: SakuraSnowAngel83@protonmail.com

## Unveil
- Check out commit history and find ETH repository
- Check commit history and discover ETH wallet address:0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef
- Check etherscan to find out the rest

## Taunt
- https://raw.githubusercontent.com/OsintDojo/public/main/deeppaste.png
- Link: http://depasteon6cqgrykzrgya52xglohg5ovyuyhte3||7hzix 7h5ldfqsyd.onion/show.php?md5=0a5c6e136a98a60b8a21643ce8c15a74
- BSSID using wigle to advance search;  84:AF:EC:34:FC:F8


What airport is closest to the location the attacker shared a photo from prior to getting on their flight?
- DCA
What airport did the attacker have their last layover in?
- HND
What lake can be seen in the map shared by the attacker as they were on their final flight home?
- Lake Inawashiro
What city does the attacker likely consider "home"?
- Hirosaki (check ssid and bssid on wigle)