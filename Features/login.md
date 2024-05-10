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
