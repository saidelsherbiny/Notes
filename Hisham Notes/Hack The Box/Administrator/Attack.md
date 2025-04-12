nmap scan
![[../../Pasted image 20250313142600.png]]


Domain name:
![[../../Pasted image 20250313142631.png]]


No interesting Shares were available:
![[../../Pasted image 20250313144125.png]]

Checking winrm access:
![[../../Pasted image 20250313144145.png]]

we have access to the machine, so let's connect using evil-winrm:
![[../../Pasted image 20250313144210.png]]



Let' deploy our Bloodhound script:
user Ethan has DCSync rights, so that is something to keep in mind:
![[../../Pasted image 20250313144901.png]]


Olivia has full control over Michael's account
![[../../Pasted image 20250313145625.png]]

And Michael can change Benjamin's Password:
![[../../Pasted image 20250313151142.png]]

So let's reset Michael's password to: p@ssword1
And then connect as Michael through winrm and reset Benjamin's password

![[../../Pasted image 20250313152443.png]]

There was an ftp server, let's try connecting to it using the known credentials:
![[../../Pasted image 20250313152533.png]]


Let's copy all the data

a psafe3 file is essentially a password file
we can try cracking the password using john, first convert the file to a hash john can use, then crack it:
![[../../Pasted image 20250313153249.png]]


Boom, that wasn't that bad 
Once in we see 3 passwords 
![[../../Pasted image 20250313153317.png]]

Emily has Genericwrite over Ethan, and Ethan has DCsync rights. We can do a DCSync Attack!
![[../../Pasted image 20250313153421.png]]

![[../../Pasted image 20250313153636.png]]

Now since Emily has genericwrite, we can use the account to 