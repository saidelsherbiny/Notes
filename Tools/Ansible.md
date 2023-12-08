If you have an encrypted password in an ansible vault:
the fata might look like this:
```
$ANSIBLE_VAULT;1.1;AES256
313563383439633230633734353632613235633932356333653561346162616664333932633737363335616263326464633832376261306131303337653964350a363663623132353136346631396662386564323238303933393362313736373035356136366465616536373866346138623166383535303930356637306461350a3164666630373030376537613235653433386539346465336633653630356531
```

one possible spots to look is: PWM/defaults/main.yml
```Bash
ansible2john passwords.txt 
```

This will give us the  ansible vault password which can now be cracked using:
```bash
ansible-vault view vault.txt
```