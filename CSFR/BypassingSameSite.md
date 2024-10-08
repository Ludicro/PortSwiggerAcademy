# Bypassing SameSite Notes

## GET vs POST Requests
```
In practicality, servers care less if they get GET or POST requests to an endpoint.

If they use Lax restrictions, CSRF requests can be done using GET requests. 
    Even if normal GET requests don't work, some frameworks provide ways of overriding in the request line
``` 

## On-site Gadgets

Some on-site gadgets can be used to bypass SameSite restrictions
    
    Example: A client-side redirect that dynamically constructs the redirection target that uses attacker-controllable input like url parameters

    From the browser's perspective, these are not really redirects, they are treated like standalone requests. 

    It is also a same-site request, so the cookies related to the site will be included. 

    Note: this will not work with server-side redirects.


## Via Vulnerable Sibling Domains
It is important to note that a request can be same-site even if it has been issued cross-origin.

Look at sibling domains that may let you send secondary requests to the target site through vulnerabilities such as XSS. 

Along with classic CSFR, you can look for WebSockets to do Cross-Site WebSocket Hijacking (CSWSH). 