# idor & broken access control

## testing workflow
1. register multiple accounts for testing purposes
2. locate a session identifier and other cookies that is important
	- could be in request headers, body requests or even the url
	- simplify the headers, requests or the url until you can't longer get data for the user
	- use developer tools and check if cookies has HttpOnly flag (usually cookies that is important have the flag)

## tips & tricks
***1. a parameter (resetPasswordUrlPrefix, urlForwarder, etc..) during reseting passwords***
- try supply a payload from burp collaborator
- check for DNS/HTTPS interactions? click on password reset link and check if token is leaked within interactions
  
***2. forging a password reset token***
- find out which part of the token is static and what changes?
- can the changed part be bruteforce?
- make the duration between 2 requests of password reset token creation shorter (use intruder), maybe the characters are going to be more predictable
  
***3. leak password reset token via Host header injection***
- add a new header with a payload from burp collaborator (any domain that you control)
	- Host: bing.com
	- X-Forwarded-Host: bing.com
- check for DNS/HTTPS interactions? click on password reset link and check if token is leaked within interactions

***4. accessing unprotected/hidden administrative endpoints***
- look for admin paths in common web files
	- robots.txt, sitemap.xml, .well-known/security.txt
	- waybackurls, hakrawler
	- commented lines in HTML source code, JavaScript code
- what determines a user can access or not? sessionId, access token?
- what is the HTTP status code?
  
***5. hidden parameter leads to privilege escalation***
- /login, /profile or any page with ability to interact with user's data
- fuzz for hidden parameters in the url or request body using arjun
	- ?role=admin, ?admin=True, ?priv=high
 
***6. bypass deny access when accessing authenticated path***
- try supply a new header like: X-Original-URL or X-Rewrite-URL
```
GET /admin HTTP/1.1			403 Denied Access
Session: normalusertoken
```
```
GET / HTTP/1.1
X-Original-URL: /admin			200 Access Granted
Session: normalusertoken
```
- making use of path discrepancies
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

***7. bypass unauthorized action using different HTTP method***
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

***8. userID is not incremental but GUIDs***
- look for a feature that involve other user's participation
- E.g: /blog, /imageContribution, /comments, /chatbox
- make a request and see if the GUIDs of other users are disclosed in the response
  
***9. data leakage upon redirect***
- change userID of other users and gets redirected to /login page
- check for disclosed user information in the 302 response page
  
***10. password disclosure (masked input) in /passwordChange page***
- in your own account, go to /passwordChange and notice your current password is visible in masked input
- reading the source code may reveal the masked password
- combine with IDOR to get administrator's password for ATO
  
***11. skip multi-step process***
- simulate the multi-step process to understand how it works
- if there are 3 steps involved, see if you can skip the 2nd process and goes directly to the 3rd step
- this typically works for business logic error as well
  
***12. bypass location-based access control***
- stumble a page that only allow users from a certain country
- use proxy or vpn and see if you can bypass deny access from other countries
