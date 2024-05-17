# testing for login function

## tips & tricks
***1. Bypass rate limit by rotating emails before reaching maximum attempts***
- intercept logon request and send it to intruder
- find out how many attempts until application blocks your request
- place the payload to both email and password
- create a wordlist for emails that will iterate the email you want to test after a couple times and set a wordlist for the password
```
testingthisemail@example.com
dummy1@example.com
dummy2@example.com
testingthisemail@example.com
```
- run the intruder and see if the email gets blocked or not

***2. No rate limit during authentication request***
- application requires email (no password) and authentication link will be sent to the email
- intercept the request and send it to intruder
- clear payloads positions and set payload types to null payloads
- run intruder and wait for emails to bomb your inbox
