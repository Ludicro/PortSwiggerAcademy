# DOM-Based XSS

## What is DOM-Based XSS?
DOM-Based XSS occurs when JavaScript takes data from attacker controlled sources, such as URLs, and passes it to a sink that supports dynamic code execution such as `eval()` or `innerHTML`. This enables the attacker to execute arbitrary JavaScript.

To deliver a DOM-Based XSS attack, the attacker must place data into a source so it can be propagated into a sink and causes the execution of arbitrary JavaScript.

The most common DOM-Based XSS is in the URL, typically accessed with the `window.location` object. Attackers can construct a link to a vulnerable page with a payload in a query string and fragment portions of the URL. 

In certain circumstances, the payload can be placed in the path. 
## How to Test for DOM-Based XSS
Majority of DOM-Based XSS can be found with BurpSuite Pro's web scanner.

Manually: look in a browser's developer tools and work through each source and test them individually.

### Test HTML Sinks
To test for DOM XSS in HTML sinks, place a random alphanumeric string into the source (such as `location.search`), then use developer's tools to inspect the HTML and find where the given string appears. 
```
Note: browser's "View Source" will not work because it does not take into account changes done in the HTML by Javascript.
```
For each location where the string appears, identify the context. Based on this, refine the input. For example, if the input appears in double quotes(""), then try injecting double quotes in your string to break out of this. 
```
Note: Browsers behave differently with regards to URL-encoding, Chrome, Firefox, and Safari will URL-encode location.search and location.hash, while IE11 and Microsoft Edge (pre-Chromium) will not URL-encode these sources. If your data gets URL-encoded before being processed, then an XSS attack is unlikely to work. 
```

### Test JavaScript Sinks
This method is usually a little harder as the input doesn't always appear in the DOM. Instead, you will have to use the Javascript debugger to find if and how the input is sent to the sink. 

For each possible source, such as `location`, you need to find cases where the source is being referenced in the code.
```
Note: You can use CTRL+Shift+F to search for a string in the page's Javascript code in Chrome.
```
Once found, you can use the Javascript debugger to add a point break and follow how the source's value is being used. The source may get assigned to different variables, if this is the case, you will also need to track those variables and see if they are passed to a sink. 

When you find a sink that is being assigned data from the source, you can use the debugger to inspect the value by hovering over the variable to show its value before it is sent to the sink. Then refine the input to see if you can deliver a successful XSS attack.

### Test for DOM XSS Using DOM Invader
Identifying and exploiting DOM XSS in the wild can be a tedious process, often requiring you to manually trawl through complex, minified JavaScript. If you use Burp's browser, however, you can take advantage of its built-in [DOM Invader](https://portswigger.net/burp/documentation/desktop/tools/dom-invader) extension, which does a lot of the hard work for you. 

## Exploiting DOM XSS with Different Sources and Sinks
In practice, sources and sinks have different properties that can impact the exploitability and determine what techniques are needed. The script may also perform validation or process data in other ways. 

There are a number of sinks that can lead to DOM-based vulnerabilities that can be found in this [list](https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities)

For example, the `document.write` sink works with `script` elements so simple payloads like `document.write('... <script>alert(document.domain)</script> ...');` can work.

[[Releveant Lab]](/XSS/Lab2_DOMXSSin_document-write_sink/Solution.md)

```
Note: Some situations the content in the document.write has surrounding context that needs to be taken into account. For example, closing existing elements.
```
[[Relevant Lab]](/XSS/Lab3_DOMXSSin_document-write_sink/Solution.md)

The `innerHTML` sink doesn't accept `script` elements on modern browsers, nor does `svg onload` elements fire. This means alternate methods like `img` or `iframe` elements need to be used. `onload` or `onerror` can be used in conjection. For example:
```
element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'
```
[[Relevant Lab]](/XSS/Lab4_DOMXSSin_innerHTML_sink/Solution.md)

### Sources and Sinks in 3rd Party Dependencies
Modern web applications are typically built using a number of third-party libraries and frameworks, which often provide additional functions and capabilities for developers. It's important to remember that some of these are also potential sources and sinks for DOM XSS. 

### DOM XSS in jQuery
If Javascript libraries such as jQuery are used, look out for sinks that can alter DOM elements. For example, jQuery's `attr()` function can change attributes of DOM elements. If data is read from a user-controlled source like the URL and is then passed to `attr()`, it is possible to inject malicious Javascript.

For example, here is JS that changes an anchor element's `href` attribute using URL data:
```
$(function() {
	$('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl'));
});
```
You can exploit this by modifying a URL so that the `location.source` contains a malicious JS URL. After the page's JavaScript applies this malicious URL to the back link's `href`, clicking the back link will execute the malicious JS.
```
?returnUrl=javascript:alert(document.domain)
```

[[Relevant Lab]](/XSS/Lab5_DOMXSS_jQueryAnchor_href/Solution.md)

Another potential sink to look out for is jQuery's `$()` selector function, which can be used to inject malicious objects into the DOM. 

jQuery used to be extremely popular, and a classic DOM XSS vulnerability was caused by websites using this selector in conjunction with the `location.hash` source for animations or auto-scrolling to a particular element on the page. This behavior was often implemented using a vulnerable `hashchange` event handler, similar to the following: 
```
$(window).on('hashchange', function() {
	var element = $(location.hash);
	element[0].scrollIntoView();
});
```
 As the `hash` is user controllable, an attacker could use this to inject an XSS vector into the `$()` selector sink. More recent versions of jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with a hash character (`#`). However, you may still find vulnerable code in the wild.

To actually exploit this classic vulnerability, you'll need to find a way to trigger a `hashchange` event without user interaction. One of the simplest ways of doing this is to deliver your exploit via an `iframe`:
```
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">
```
In this example, the `src` attribute points to the vulnerable page with an empty hash value. When the `iframe` is loaded, an XSS vector is appended to the hash, causing the `hashchange` event to fire. 

Note:
Even newer versions of jQuery can still be vulnerable via the `$()` selector sink, provided you have full control over its input from a source that doesn't require a `#` prefix. 

### DOM XSS in AngularJS
If a framework like AngularJS is used, it is possible to execute JS without angle brackets or events. When a site uses the `ng-app` attribute on an HTML element, it will be processed by AngularJS. In this case, AngularJS will execute JavaScript inside double curly braces that can occur directly in HTML or inside attributes.

## DOM XSS Combined with Reflected and Stored Data
Some pure DOM-based vulnerabilities are self-contained within a single page. If scripts read data from a URL and writes it to a dangerous sink, then the vulnerability is entirely client-side.

However, sources aren't limited to data that is directly exposed by browsers - they can also originate from the website. For example, websites often reflect URL parameters in the HTML response from the server. This is commonly associated with normal XSS, but it can also lead to reflected DOM XSS vulnerabilities.

In a reflected DOM XSS vulnerability, the server processes data from the request, and echoes the data into the response. The reflected data might be placed into a JavaScript string literal, or a data item within the DOM, such as a form field. A script on the page then processes the reflected data in an unsafe way, ultimately writing it to a dangerous sink. 

`eval('var data = "reflected string"');`

Websites may also store data on the server and reflect it elsewhere. In a stored DOM XSS vulnerability, the server receives data from one request, stores it, and then includes the data in a later response. A script within the later response contains a sink which then processes the data in an unsafe way. 

`element.innerHTML = comment.author`

## Which Sinks Lead to DOM XSS Vulnerabilities?


## How to Prevent DOM XSS