# What is DOM-Based XSS?
DOM-Based XSS occurs when JavaScript takes data from attacker controlled sources, such as URLs, and passes it to a sink that supports dynamic code execution such as `eval()` or `innerHTML`. This enables the attacker to execute arbitrary JavaScript.

To deliver a DOM-Based XSS attack, the attacker must place data into a source so it can be propagated into a sink and causes the execution of arbitrary JavaScript.

The most common DOM-Based XSS is in the URL, typically accessed with the `window.location` object. Attackers can construct a link to a vulnerable page with a payload in a query string and fragment portions of the URL. 

In certain circumstances, the payload can be placed in the path. 
# How to Test for DOM-Based XSS
Majority of DOM-Based XSS can be found with BurpSuite Pro's web scanner.

Manually: look in a browser's developer tools and work through each source and test them individually.

## Test HTML Sinks
To test for DOM XSS in HTML sinks, place a random alphanumeric string into the source (such as `location.search`), then use developer's tools to inspect the HTML and find where the given string appears. 
```
Note: browser's "View Source" will not work because it does not take into account changes done in the HTML by Javascript.
```
For each location where the string appears, identify the context. Based on this, refine the input. For example, if the input appears in double quotes(""), then try injecting double quotes in your string to break out of this. 
```
Note: Browsers behave differently with regards to URL-encoding, Chrome, Firefox, and Safari will URL-encode location.search and location.hash, while IE11 and Microsoft Edge (pre-Chromium) will not URL-encode these sources. If your data gets URL-encoded before being processed, then an XSS attack is unlikely to work. 
```

## Test JavaScript Sinks
This method is usually a little harder as the input doesn't always appear in the DOM. Instead, you will have to use the Javascript debugger to find if and how the input is sent to the sink. 

For each possible source, such as `location`, you need to find cases where the source is being referenced in the code.
```
Note: You can use CTRL+Shift+F to search for a string in the page's Javascript code in Chrome.
```
Once found, you can use the Javascript debugger to add a point break and follow how the source's value is being used. The source may get assigned to different variables, if this is the case, you will also need to track those variables and see if they are passed to a sink. 

When you find a sink that is being assigned data from the source, you can use the debugger to inspect the value by hovering over the variable to show its value before it is sent to the sink. Then refine the input to see if you can deliver a successful XSS attack.

## Test for DOM XSS Using DOM Invader
Identifying and exploiting DOM XSS in the wild can be a tedious process, often requiring you to manually trawl through complex, minified JavaScript. If you use Burp's browser, however, you can take advantage of its built-in [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) extension, which does a lot of the hard work for you. 

# Exploiting DOM XSS with Different Sources and Sinks
In practice, sources and sinks have different properties that can impact the exploitability and determine what techniques are needed. The script may also perform validation or process data in other ways. 

There are a number of sinks that can lead to DOM-based vulnerabilities that can be found in this [list](https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities)

For example, the `document.write` sink works with `script` elements so simple payloads like `document.write('... <script>alert(document.domain)</script> ...');` can work.

[[Releveant Lab]](/XSS/Lab2_DOMXSSin_document-write_sink/Solution.md)

```
Note: Some situations the content in the document.write has surrounding context that needs to be taken into account. For example, closing existing elements.
```

The `innerHTML` sink doesn't accept `script` elements on modern browsers, nor does `svg onload` elements fire. This means alternate methods like `img` or `iframe` elements need to be used. `onload` or `onerror` can be used in conjection. For example:
```
element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'
```

## Sources and Sinks in 3rd Party Dependencies

## DOM XSS in jQuery

## DOM XSS in AngularJS

# DOM XSS Combined with Reflected and Stored Data

# Which Sinks Lead to DOM XSS Vulnerabilities?

# How to Prevent DOM XSS