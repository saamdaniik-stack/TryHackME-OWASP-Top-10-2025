# TryHackME OWASP Top 10 2025

## IAAA Failures

### What does IAAA stand for?

`Identity, Authentication, Authorisation, Accountability`
### A01: Broken Access Control

1. If you don't get access to more roles but can view the data of another users, what type of privilege escalation is this?

`Answer: Horizontal`

2. What is the note you found when viewing the user's account who had more than $ 1 million?

`Flag: THM{Found.the.Millionare!}`

#### Writeup:

The site has an IDOR vulnerability—changing the ID from 5 to 7 allows access to another user's profile where the flag is hidden.

<img width="651" height="499" alt="image" src="https://github.com/user-attachments/assets/3f22ccbf-f1ba-4d91-af08-edad00594eef" />

### A07: Authentication Failures

1. What is the flag on the admin user's dashboard?

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

## Application Design Flaws

### AS02: Security Misconfigurations

#### Challenge

Navigate to 10.48.132.192:5002. It appears that the developers left too many traces in their User Management APIs.

Flag: `THM{V3RB0S3_3RR0R_L34K}`

Directory bruteforcing /api endpoints reveals few other directories. In /api/user/admin the flag can be retrieved.

<img width="1365" height="684" alt="image" src="https://github.com/user-attachments/assets/8df9d443-d610-46f4-ba30-740756e82b32" />

<img width="1365" height="642" alt="image" src="https://github.com/user-attachments/assets/33039eb6-b77e-4cc2-87e2-2801f11a8dec" />

### AS03: Software Supply Chain Failures

#### Challenge

Navigate to 10.48.132.192:5003. The code is outdated and imports an old lib/vulnerable_utils.py component. Can you debug it?

We have two endpoints to hit here, one is `/api/health` and `/api/process`.

<img width="1365" height="599" alt="image" src="https://github.com/user-attachments/assets/a0bacb27-45ef-46a5-a139-2acb303296b0" />

`/api/health` - Doesn't seem interesting

<img width="1365" height="596" alt="image" src="https://github.com/user-attachments/assets/7d22ed5a-2be1-461a-94a7-b781bb20e0fe" />

Let’s hit /api/process. Make sure to include the Content-Type: application/json header, since it’s a RESTful API that works only with JSON. Also, it’s a POST request, so it requires a parameter which is data.

<img width="593" height="427" alt="image" src="https://github.com/user-attachments/assets/b1d3b143-7535-4866-a60f-0e80fb045898" />


`if data == 'debug': # Value Leaked Here
            return jsonify(debug_info())`

So our value is debug, which reveals the flag.

<img width="1365" height="571" alt="image" src="https://github.com/user-attachments/assets/45d0a0b7-ad6a-4a96-ba4a-3e7bfb230874" />

Flag: `THM{SUPPLY_CH41N_VULN3R4B1L1TY}`


### AS04: Cryptographic Failures

`Nzd42HZGgUIUlpILZRv0jeIXp1WtCErwR+j/w/lnKbmug31opX0BWy+pwK92rkhjwdf94mgHfLtF26X6B3pe2fhHXzIGnnvVruH7683KwvzZ6+QKybFWaedAEtknYkhe`
<img width="1365" height="528" alt="image" src="https://github.com/user-attachments/assets/b797e637-d62a-4089-b35f-5b40036baafe" />

Anaylsing the source code we can see a js file called decrypt.js which revealed the secret key and algorithm of the encryption

<img width="1365" height="604" alt="image" src="https://github.com/user-attachments/assets/759996f6-b01e-4d46-9184-9914061345ca" />

Now lets decrypt it using decryptors in online like `https://www.devglan.com/online-tools/aes-encryption-decryption`

<img width="1365" height="680" alt="image" src="https://github.com/user-attachments/assets/fd4ecf32-25eb-4130-95dc-7f95bec99955" />

Flag: `THM{CRYPTO_FAILURE_H4RDCOD3D_K3Y}`

### AS06: Insecure Design

Hitting `/api/users/admin` this endpoint reveals

<img width="1365" height="596" alt="image" src="https://github.com/user-attachments/assets/cfd464b5-13f3-4f12-8629-5928a5ee61a7" />

We can see /api/users/admin gives some data lets try fuzzing the endpoint after /api/ with /admin maybe we can get endpoints like `/api/profile/admin` or `/api/data/admin`

Fuzzing with ffuf:

Command:

`ffuf -u http://10.49.154.138:5005/api/FUZZ/admin -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt`

We get /messages endpoints which reveals the flag:

<img width="1365" height="640" alt="image" src="https://github.com/user-attachments/assets/741dca31-4c94-4e00-b31e-620b3de4bfb7" />

Flag: `THM{1NS3CUR3_D35IGN_4SSUMPT10N}`

## Insecure Data Handling
