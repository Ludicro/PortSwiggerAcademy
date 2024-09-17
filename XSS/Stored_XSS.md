# What is Stored XSS?
Occurs when an application recieves data from an untrusted source and includes that data within the HTTP response in an unsafe way.

If a site allows for users to post comments on a blogpost which is displayed to other users, the HTTP request may look like the following:
```
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net
```

If the application does not validate any data an attack can take the following malicious comment like:
```
<script>/* Bad stuff here... */</script>
```
And translate it to:
```
comment=%3Cscript%3E%2F*%2BBad%2Bstuff%2Bhere...%2B*%2F%3C%2Fscript%3E
```
This will result in the anyone visiting that blogpost getting the following response:
```
<p><script>/* Bad stuff here... */</script></p>
```

# Impact of Stored XSS
If an attacker can control a script that gets executed in a user's browser, they can usually fully compromise the user and carry out any actions that can be done by a reflected XSS attack. 

The benefit of this attack over reflected XSS is that the attacker does not need to find an external way to get the victim to send the bad request.

The attacker simply stores their attack in the application and waits for users to encounter it. 

However, if the vulnerability can be executed on users who are not signed in, the attack is not as effective as no special permissions or access is gained. 