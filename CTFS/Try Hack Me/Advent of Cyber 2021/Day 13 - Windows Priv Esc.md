# Day 13 - Networking - Windows PrivEsc

## Different Windows Privileges
-   Domain Administrators: This is typically the highest account level you will find in an enterprise along with Enterprise Administrators. An account with this level of privilege can manage all accounts of the organization, their access levels, and almost anything you can think of. A "domain" is a central registry used to manage all users and computers within the organization.
-   Services: Accounts used by software to perform their tasks such as back-ups or antivirus scans.
-   Domain users: Accounts typically used by employees. These should have just enough privileges to do their daily jobs. For example, a system administrator may restrict a user's ability to install and uninstall software.
-   Local accounts: These accounts are only valid on the local system and can not be used over the domain.

![](https://i.imgur.com/RlLekym.png)

## Common enum points:

- `net users`
- `systeminfo | findstr /B /C: "OS Name"/C: "OS Version"`
- `wmic service list`