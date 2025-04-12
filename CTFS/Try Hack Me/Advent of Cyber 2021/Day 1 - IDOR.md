# Day 1 - Web - IDOR
[web]
## What is an IDOR Vulnerability?

- Insecure Direct Object Reference is a type of access control vulnerability.
- Allows an attacker to gain access to information or actions which are not intended for them.
- An IDOR occurs when user supplied data to retrieve objects from the server which does not validate whether the user should actually have access to the requested object.

## How do I find and exploit IDOR vulnerabilities? 

### Query component in URL
- Changing the `?id=` component or similar in the URL to access information not meant for the user

### Post variables
- Changing you password with a post request may be done insecurely
![](https://i.imgur.com/2b6IrrO.png)

### Cookies
- Changing cookie values allowing access to other users 

## IDOR in the wild

https://corneacristian.medium.com/top-25-idor-bug-bounty-reports-ba8cd59ad331

