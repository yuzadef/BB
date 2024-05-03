# IDOR & Broken Access Control (BAC)

## testing workflow
1. register multiple accounts for testing purposes
2. locate a session identifier and other cookies that is important
	- could be in request headers, body requests or even the url
	- simplify the headers, requests or the url until you can't longer get data for the user
	- use developer tools and check if cookies has HttpOnly flag (usually cookies that is important have the flag)

## tips & tricks
1. a parameter (resetPasswordUrlPrefix, urlForwarder, etc..) during reseting passwords
	- try supply a payload from burp collaborator
	- check for DNS/HTTPS interactions? click on password reset link and check if token is leaked within interactions

2. forging a password reset token
	- find out which part of the token is static and what changes?
	- can the changed part be bruteforce?
	- make the duration between 2 requests of password reset token creation shorter (use intruder), maybe the characters are going to be more predictable

3. leak password reset token via Host header injection
	- add a new header with a payload from burp collaborator (any domain that you control)
		- Host: bing.com
		- X-Forwarded-Host: bing.com
	- check for DNS/HTTPS interactions? click on password reset link and check if token is leaked within interactions

4. privilege escalation from user to admin
	- find out what roles exists and what they can do
	- how a user can claim a role & can it be tempered
		- HTTP request body or url parameters
		- specified in access token or authorization token
		- HTTP headers

5. accessing unprotected/hidden administrative endpoints
	- look for admin paths in common web files
		- robots.txt, sitemap.xml, .well-known/security.txt
		- waybackurls, hakrawler
		- commented lines in HTML source code, JavaScript code
	- what determines a user can access or not? sessionId, access token?
	- what is the HTTP status code?

6. hidden parameter leads to privilege escalation
	- /login, /profile or any page with ability to interact with user's data
	- fuzz for hidden parameters in the url or request body using arjun
		- ?role=admin, ?admin=True, ?priv=high

7. bypass deny access for unauthenticated user when accessing authenticated path (PATH based)
	- try supply a new header like: X-Original-URL or X-Rewrite-URL
	```
	GET /admin HTTP/1.1
	Host: www.example.com 				403 Denied Access
	Session: normalusertoken
	```
	```
	GET / HTTP/1.1
	X-Original-URL: /admin          	200 Access Granted
	Session: normalusertoken
	```
