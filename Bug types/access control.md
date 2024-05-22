# idor & broken access control

## tips & tricks

***1. accessing unprotected/hidden administrative endpoints***
- look for admin paths in common web files
	- robots.txt, sitemap.xml, .well-known/security.txt
	- waybackurls, hakrawler
	- commented lines in HTML source code, JavaScript code
- what determines a user can access or not? sessionId, access token?
- what is the HTTP status code?
  
***2. hidden parameter leads to privilege escalation***
- /login, /profile or any page with ability to interact with user's data
- fuzz for hidden parameters in the url or request body using arjun
	- ?role=admin, ?admin=True, ?priv=high
 
***3. bypass deny access when accessing authenticated path***
- supply a new header like: X-Original-URL or X-Rewrite-URL
```
GET /admin HTTP/1.1			403 Denied Access
Session: normalusertoken
```
```
GET / HTTP/1.1
X-Original-URL: /admin			200 Access Granted
Session: normalusertoken
```
- path discrepancies
```
/admin 	      403 Denied Access >> /ADMIN 	200 Access Granted
/admin/files 	403 Denied Access >> /admin/files/ 	200 	Access Granted
/admin/deleteUser	403 Unauthorized >> /admin/deleteUser.php	200 Success
```
- bypass deny access via referer header
```
GET /admin/deleteUser HTTP/1.1
Session: normalusertoken		403 Denied Access
Referer: /profilePage
```
```
GET /admin/deleteUser HTTP/1.1
Session: normalusertoken		200 Access Granted
Referer: /admin
```

***4. bypass unauthorized action using different HTTP method***
- instead of POST, try GET or PUT
```
POST /admin-roles HTTP/1.1
Session: normalusertoken		403 Unauthorized

username=normaluser&action=upgrade
```
```
GET /admin-roles?username=normaluser&action=upgrade		200 Success
Session: normalusertoken
```
  
***5. skip multi-step process***
- simulate the multi-step process to understand how it works
- if there are 3 steps involved, see if you can skip the 2nd process and goes directly to the 3rd step
- this typically works for business logic error as well
  
***6. bypass location-based access control***
- stumble a page that only allow users from a certain country
- use proxy or vpn and see if you can bypass deny access from other countries

***7. leak password reset poisoning via parameter tampering***
- a parameter (resetPasswordUrlPrefix, urlForwarder, etc..) during requesting for password reset link
- supply a payload from burp collaborator to the parameter and check for DNS/HTTPS interactions when clicking on password reset link. Check if token is leaked within interactions

***8. forging a password reset token***
- find out which part of the token is static and what changes?
- can the changed part be bruteforce?
- make the duration between 2 requests of password reset token creation shorter (use intruder), maybe the characters are going to be more predictable

***9. password reset poisoning via host header***
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

***10. password reset token leaked in referer header***
- request for password reset link
- in the password reset page, click on any social media link then intercept the request
- notice the password reset token is disclosed in the referer header
<details>
<summary>writeups</summary>
	
* [342693](https://hackerone.com/reports/342693)
* [272379](https://hackerone.com/reports/272379)
  
</details>

***11. polluting parameter to get password reset link***
- request for password reset link with other user email and intercept the request
- add another email parameter with your own email and send the request
- the password reset link will be sent to both victim and you
<details>
<summary>writeups</summary>
	
* [Readme.com ATO](https://medium.com/@0xankush/readme-com-account-takeover-bugbounty-fulldisclosure-a36ddbe915be)
  
</details>
