# TryHackME OWASP Top 10 2025

## IAAA Failures

### What does IAAA stand for?

`Identity, Authentication, Authorisation, Accountability`
### A01: Broken Access Control

1. If you don't get access to more roles but can view the data of another users, what type of privilege escalation is this?

`Answer: Horizontal`
3. What is the note you found when viewing the user's account who had more than $ 1 million?

`Flag: THM{Found.the.Millionare!}`
#### Writeup:

The site has an IDOR vulnerability—changing the ID from 5 to 7 allows access to another user's profile where the flag is hidden.

<img width="651" height="499" alt="image" src="https://github.com/user-attachments/assets/3f22ccbf-f1ba-4d91-af08-edad00594eef" />

### A07: Authentication Failures

1. What is the flag on the admin user's dashboard?
2. 
`Flag: THM{Account.confusion.FTW!}`

#### Writeup:

Register a new account using the username aDmiN to bypass case sensitivity, then log in as the admin user and retrieve the flag.

<img width="665" height="633" alt="image" src="https://github.com/user-attachments/assets/d0217540-87c7-4aa2-8101-cc77f9a481f7" />

### A09: Logging & Alerting Failures

1. It looks like an attacker tried to perform a brute-force attack, what is the IP of the attacker?

`Answer: 203.0.113.45`

2. Looks like they were able to gain access to an account! What is the username associated with that account?

`Answer: admin`

3. What action did the attacker try to do with the account? List the endpoint the accessed.

`Answer: /supersecretadminstuff`

<img width="607" height="533" alt="image" src="https://github.com/user-attachments/assets/cac573a3-8487-418c-94bf-9913fcdc1fce" />



