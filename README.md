# Personal repository for reference during testing

- Contains custom payloads tested throughout testing journey

- Tips & tricks collected from other hunters reports

- My own hunting reports

## What to test?

***Tags: password reset, account takeover, authentication bypass, OTP***
1. no rate limit on password reset request leads to mass mailing

2. link poisoning on password reset request leads to ATO

3. leaked password reset token via host header injection leads to ATO

4. authentication bypass via response manipulation leads to ATO

5. OTP bypass via response manipulation leads to ATO

***Tags: session identifier, application logic, misconfiguration***

1. insufficient session expiration after logout



## Resources

- https://github.com/daffainfo/AllAboutBugBounty/tree/master

- https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master

- https://www.bugbountyhunting.com/

- https://pentester.land/writeups/

- https://github.com/reddelexc/hackerone-reports/tree/master
