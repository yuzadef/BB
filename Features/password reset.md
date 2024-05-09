# testing for password reset 

## tips & tricks
***1. password reset poisoning via parameter tampering***
- a parameter (resetPasswordUrlPrefix, urlForwarder, etc..) during requesting for password reset link
- supply a payload from burp collaborator to the parameter and check for DNS/HTTPS interactions when clicking on password reset link. Check if token is leaked within interactions
  
***2. forging a password reset token***
- find out which part of the token is static and what changes?
- can the changed part be bruteforce?
- make the duration between 2 requests of password reset token creation shorter (use intruder), maybe the characters are going to be more predictable
  
***3. password reset poisoning via host header***
- add a new Host header with a controlled domain and see if token is leaked within HTTP interactions
```
GET /passwordResetLink HTTP/1.1
Host: example.com
Host: burpcollaborator.io	[1]
X-Forwarded-Host: burpcollaborator.io	[2]
```
- make a GET request to absolute url but replace the Host header value and see if token is leaked within HTTP interactions
```
GET https://example.com/passwordResetLink HTTP/1.1
Host: burpcollaborator.io
```
<details>
<summary>writeups</summary>
	
* [Acunetix article](https://www.acunetix.com/blog/articles/password-reset-poisoning/)
  
</details>
  
***4. skip multi-step process***
- simulate the multi-step process to understand how it works
- if there are 3 steps involved, see if you can skip the 2nd process and goes directly to the 3rd step
- this typically works for business logic error as well

***5. password reset token leaked in referer header (H1: 342693/272379)***
- request for password reset link
- in the password reset page, click on any social media link then intercept the request
- notice the password reset token is disclosed in the referer header
<details>
<summary>writeups</summary>
	
* [342693](https://hackerone.com/reports/342693)
* [272379](https://hackerone.com/reports/272379)
  
</details>

***6. polluting parameter to get password reset link***
- request for password reset link with other user email and intercept the request
- add another email parameter with your own email and send the request
- the password reset link will be sent to both victim and you
<details>
<summary>writeups</summary>
	
* [Readme.com ATO](https://medium.com/@0xankush/readme-com-account-takeover-bugbounty-fulldisclosure-a36ddbe915be)
  
</details>

***7. no rate limit on password reset page leads to mass mailing***
- request for password reset link and intercept the request
- send the request to intruder and set a high payload value (e.g: 100 numbers)
- start the attack and see that your email inbox is bombarded with 100s of emails
<details>
<summary>writeups</summary>
	
* [1166066](https://hackerone.com/reports/1166066)
* [751604](https://hackerone.com/reports/751604)
  
</details>