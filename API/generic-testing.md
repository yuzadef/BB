- [ ] check for API documentations, references, github commits, known vulnerabilities
- [ ] click on all links, inputs where you can, understand the app | get all api endpoints/features > fire up bs to capture requests
- [ ] look for keywords in JS source files: api, apiKey, secret, token, url, paths
- [ ] run kiterunner/wfuzz in the background (scan or brute) for api path enumeration
- [ ] interact with APIs directly | understanding the intended usage (request, response, headers, authorization, authentication)
```
  • What sorts of actions can I take?
  • Can I interact with other user accounts?
  • What kinds of resources are available?
  • When I create a new resource, how is that resource identified?
  • Can I upload a file? Can I edit a file?
  • How does the API determine which users?
  • response baseline | comparison with successful and bad response
```
- [ ] interact with APIs with unintended usage (methods, removing token, adding multiple parameters or http headers, unexpected inputs)
```
  • pay attention to the response
  • what is allowed and what is not?
  • what happens if you do this & that?
  • expects email? try numbers | expects numbers? try strings | expects boolean? try anything
    • gradually change valid input until it does not work
  • try to escape input sanitization using escape characters (null byte, pipe, quotes, spaces)
    • test@email.com%00payload
  • skipping requests (payment, account creation, reset password)
  • changing request methods, removing cookies or tokens
```

- [ ] fuzzing wide & fuzzing deep the inputs, headers, authentication value, request body
```
	• database queries | system commands
	• a string of letters when number is expected | longer strings when shorter strings is expected
	• various special characters | emoticons | characters from unexpected language (e.g: chinese)
	• request methods
	• improper assets management (v1, v2, internal, dev, non-prod, mobile, uat, old)
	• fuzz for escape characters | null bytes, traversal characters, encoded characters
	• fuzz for directory traversal
```
- [ ] apply evasive techniques to endpoint that caught your attention
