# Login/Register functionality testing

## Tips & tricks

***1. No rate limit on authentication token link request leads to mass mailing/mail bombings***
- intercept the auth token link request and send to Intruder
- clear all payloads positions and set the payload type to Null
- start the attack and observe hundreds of auth token link emails in your inbox
