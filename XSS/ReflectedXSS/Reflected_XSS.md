# Reflected XSS

## What is Reflected XSS?
Reflected XSS occurs when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way.

For example, a website has a search function that takes the search term and supplies it to the URL:
```
https://insecure-website.com/search?term=gift
```
And the result on the page looks like:
```
<p>You searched for: gift</p>
```

An attack on this may look like:
```
https://insecure-website.com/search?term=<script>HarmfulCode</script>
```
Which turns into:
```
<p>You searched for: <script>HarmfulCode</script></p>
```

If another user visits the URL crafted by the attacker, the script will execute in the victim's browser. 

## Impact of Reflected XSS
If an attacker can successfully exploit a reflected XSS vulnerability, they can:
- Perform actions that the user is able to perform.
- Read any data that the user is able to access.
- Modify information the user has permissions to modify.
- Initiate interactions with other application users, including malicious attacks, the will appear to originate from the inital victim.
