# XSS

## What is Cross-Site Scripting (XSS)?
XSS is a web vulnerability that allows attackers to compromise interactions users have with vulnerable applications. 

It allows attackers to avoid the same-origin policy which is designed to segregate different sites from each other. 

XSS allows attackers to "assume" the identity of a user, carry out actions as that user, and access their data. This is extremely useful if a user is privileged on a site.

## How Does XSS Work?
XSS manipulates vulnerable sites to return malicious JS to users. 

## XSS Proof Of Concept
XSS code can be confirmed by injecting payloads that cause your own browser to execute the Javascript. 

Commonly, the `alert()` function is used because it's easy to use and the result is obvious. 

However, with modern Chrome, cross-origin iframes are prevented from using the `alert()` function, which means that more advanced payloads are required using different PoC such as the `print()` function. 

## Types of XSS 
- Reflected XSS
  - Malicious script comes from HTTP request
- Stored XSS
  - Malicious script comes from the database
- DOM-based XSS
  - Vulnerability exists in the clientt-side code

### Reflected XSS
Simplest form of XSS is [Reflected XSS](Reflected_XSS). 

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

### Stored XSS
When an application recieves data from an untrusted source and includes that data within HTTP responses in unsafe ways.

Data may be submitted in the form of comments, usernames, contact details, etc. 

It may also come from other untrusted sources such as a webmail app displaying messages, marketing application for showing social media posts, or network monitoring app displaying traffic data.

Here is a simple example of a stored XSS vulnerability. A message board application lets users submit messages, which are displayed to other users:

```
<p>Hello, this is my message!</p>
```

The application doesn't perform any other processing of the data, so an attacker can easily send a message that attacks other users:

```
<p><script>/* Bad stuff here... */</script></p>
```

### DOM Based XSS
When an app contain client-side code that processes untrusted data.

In the following example, an application uses some JavaScript to read the value from an input field and write that value to an element within the HTML:
```
var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;
```

If the attacker can control the value of the input field, they can easily construct a malicious value that causes their own script to execute:
```
You searched for: <img src=1 onerror='/* Bad stuff here... */'>
```

Usually, the input field is populated in part of the HTTP request, allowing an attacker to deliever an attack from a malicious URL, similarly to [reflected XSS](#reflected-xss).


## What is XSS Used For?
Attackers who exploit XSS vulnerabilities can usually do the following:
- Impersonate or masquerade as the victim user.
- Carry out any action that the user is able to perform.
- Read any data that the user is able to access.
- Capture the user's login credentials.
- Perform virtual defacement of the web site.
- Inject trojan functionality into the web site.

## Finding and Testing for XSS
Automatically: BurpSuite Pro's web vulnerability scanner

Manually: 
1. Submit input into entry points in the application
2. Identify where the input is returned in HTTP responses
3. Test each location to determine if the input can be exploited

DOM-based Manual Testing:
1. Place input in the parameter
2. Use browser's developer tools to search DOM for the input
3. Test each input to see if it is exploitable

Alternatively, <u>review the Javascript code</u> (**this can be very time consuming**).

## Content Security Policy (CSP)
CSP is a browser mechanism which is intended to mitigate impacts of XSS and other vulnerabilities. 

CSP may hinder or prevent XSS-like activity, but can be circumvented in many cases.

## Dangling Markup Injection
A technique used to capture data cross-domain where full XSS is not viable due to input filters or other security mechanisms. 

It may be exploited to capture sensitive data that is visible to others, including [CSRF tokens](/CSFR/CSRF_Notes.md#defending-against-csrf).

## How to Prevent XSS
Effectively preventing XSS usually involves combining the following techniques:
- Filter input on arrival
  - At the point where user input is recieved, filter as strictly as possible based on what is expected or valid input.
- Encode data on output
  - At the point where user-controllable data is output in HTTP responses, encode it to prevent it from being interpreted. 
- Use appropriate response headers
  - Use `Content-Type` and `X-Content-Type-Options` headers to ensure browsers interpret responses in the way you intended.
- Content security policy 
  - Use CSP to reduce severity of XSS vulnerabilities.
