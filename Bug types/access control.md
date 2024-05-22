# idor & broken access control

## testing workflow
***1. register multiple accounts for testing purposes***

***2. locate a session identifier and other cookies that is important***
- could be in request headers, body requests or even the url
- simplify the headers, requests or the url until you can't longer get data for the user
- use developer tools and check if cookies has HttpOnly flag (usually cookies that is important have the flag)
- if the application uses JWTs
  	- determine the algorithm used
  	- tamper with the payloads to find out which parameter is being used to pull out the larger data set of a user
  	- compare the token with another user and see what differs

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
