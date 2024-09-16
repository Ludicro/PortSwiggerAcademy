# What is Cross-Site Scripting (XSS)?
XSS is a web vulnerability that allows attackers to compromise interactions users have with vulnerable applications. 

It allows attackers to avoid the same-origin policy which is designed to segregate different sites from each other. 

XSS allows attackers to "assume" the identity of a user, carry out actions as that user, and access their data. This is extremely useful if a user is privileged on a site.

# How Does XSS Work?
XSS manipulates vulnerable sites to return malicious JS to users. 

# XSS Proof Of Concept
XSS code can be confirmed by injecting payloads that cause your own browser to execute the Javascript. 

Commonly, the `alert()` function is used because it's easy to use and the result is obvious. 

However, with modern Chrome, cross-origin iframes are prevented from using the `alert()` function, which means that more advanced payloads are required using different PoC such as the `print()` function. 

# Types of XSS 
- Reflected XSS
  - Malicious script comes from HTTP request
- Stored XSS
  - Malicious script comes from the database
- DOM-based XSS
  - Vulnerability exists in the clientt-side code

## Reflected XSS
Simplest form of XSS. 

This is used when an application gets data from an HTTP request and includes it in the response.

Here is an example of a site:
```
https://insecure-website.com/status?message=All+is+well.

<p>Status: All is well.</p>
```
So an attack might look like:
```
https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>

<p>Status: <script>/* Bad stuff here... */</script></p>
```
If the user visits the URL made by the attacker, the script will be executed in the user's browser, in the context of the user's session. 

This then leads to the script to perform any action or access any data the user has access to. 

## Stored XSS