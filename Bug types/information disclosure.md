# information disclosure

## tips & tricks
***1. check commented lines in HTML source code for sensitive informations***
- use burpsuite engagement tool to automatically get all commented lines from the target sitemap
- look for anything sensitive or informational
	- default credentials
	- CMS versions or server versions
	- API, admin or any hidden paths

***2. password disclosure (masked input) in /passwordChange page***
- in your own account, go to /passwordChange and notice your current password is visible in masked input
- reading the source code may reveal the masked password
- combine with IDOR to get administrator's password for ATO

***3. user GUIds leaked in app features***
- look for a feature that involve other user's participation
- E.g: /blog, /imageContribution, /comments, /chatbox
- make a request and see if the GUIDs of other users are disclosed in the response
  
***4. data leakage upon redirect***
- change userID of other users and gets redirected to /login page
- check for disclosed user information in the 302 response page
