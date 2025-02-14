# XSS Lab 1: Reflected XSS

# Analysis
Website has a search bar that when terms are entered into it, turns the URL from:
```
https://LAB-NUM.web-security-academy.net
```
into
```
https://LAB-NUM.web-security-academy.net/?search=TERM
```

This means that user input is reflected in the URL so we can try to add some JavaScript to the search box.

Adding in `<script> alert(0) </script>` was not filtered and the code was executed as well as reflected in the URL as:
```
https://LAB-NUM.web-security-academy.net/?search=%3Cscript%3Ealert%280%29%3C%2Fscript%3E
```