# no rate limit & rate limit bypass

## tips & tricks
***1. no rate limit on password reset page leads to mass mailing***
- request for password reset link and intercept the request
- send the request to intruder and set a high payload value (e.g: 100 numbers)
- start the attack and see that your email inbox is bombarded with 100s of emails
<details>
<summary>writeups</summary>
	
* [1166066](https://hackerone.com/reports/1166066)
* [751604](https://hackerone.com/reports/751604)
</details>

***2. Bypass rate limit by rotating emails before reaching maximum attempts***
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

***3. No rate limit during authentication token link request leads to mass mailing/mail bombing***
- application requires email (no password) and authentication link will be sent to the email
- intercept the request and send it to intruder
- clear payloads positions and set payload types to null payloads
- run intruder and wait for emails to bomb your inbox
