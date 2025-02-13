# Analysis
The string inputed in the search bar was found in the following snippet from Inspecting:
```
<img src="/resources/images/tracker.gif?searchTerms=myTestWords">
```

XSS that might break out and load a script:
```
"><svg onload=alert(1)>
```