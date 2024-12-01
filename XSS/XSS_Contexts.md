# Cross-Site Scripting Contexts
When testing for reflected and stored XSS, a key task is identifying XSS context:

- The location within the response where attacker-controllable data appears. 
- Any input validation or other processing that is being performed on that data by the application. 

Based on this information, you can select one or more XSS payloads and test their effectiveness. 

# XSS Between HTML Tags
When the XSS context is between HTML tags, you need to introduce some new HTML tags designed to trigger the execution of JS. 

Useful ways of executing JS are:
```
<script>alert(document.domain)</script>
<img src=1 onerror=alert(1)>
```

## XSS in HTML Tag Attributes