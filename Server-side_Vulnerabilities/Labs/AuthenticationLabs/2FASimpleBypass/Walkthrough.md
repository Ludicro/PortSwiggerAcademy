## Authentication - Lab 2: 2FA Simple Bypass

### Description
This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

Your credentials: `wiener`:`peter`
Victim's credentials `carlos`:`montoya`


This lab is incredibly simple. When we sign in with our credentials, we are given a 2FA prompt and if we go to the fake email client the lab gives us, we have our code and can sign in. In the URL, we can see the account page is located at `/my-account?id=wiener`. 

Since we already have Carlos' credentials, when we sign in with them, we are given a 2FA prompt, but we don't have access to this email. However, if we change the URL to match the standard `/my-account?id=carlos`, we will bypass the 2FA and be taken straight to his account page.