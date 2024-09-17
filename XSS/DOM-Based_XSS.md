# What is DOM-Based XSS?
DOM-Based XSS occurs when JavaScript takes data from attacker controlled sources, such as URLs, and passes it to a sink that supports dynamic code execution such as `eval()` or `innerHTML`. This enables the attacker to execute arbitrary JavaScript.

To deliver a DOM-Based XSS attack, the attacker must place data into a source so it can be propagated into a sink and causes the execution of arbitrary JavaScript.

The most common DOM-Based XSS is in the URL, typically accessed with the `window.location` object. Attackers can construct a link to a vulnerable page with a payload in a query string and fragment portions of the URL. 

In certain circumstances, the payload can be placed in the path. 
# How to Test for DOM-Based XSS

## Test HTML Sinks

## Test JavaScript Sinks

## Test for DOM XSS Using DOM Invader

# Exploiting DOM XSS with Different Sources and Sinks

## Sources and Sinks in 3rd Party Dependencies

## DOM XSS in jQuery

## DOM XSS in AngularJS

# DOM XSS Combined with Reflected and Stored Data

# Which Sinks Lead to DOM XSS Vulnerabilities?

# How to Prevent DOM XSS