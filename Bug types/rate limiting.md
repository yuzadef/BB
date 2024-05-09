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
