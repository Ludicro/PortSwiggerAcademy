# Server-Side Request Forgery (SSRF)
> SSRF vulnerabilities enable an attacker to trigger malicious server-to-server requests to unintended URLs. As the server issuing the request is likely to have a strong trust relationship with other systems on the network, the attacker can potentially abuse this behavior to access data, functionality, and services that are not meant to be exposed to external users.

## What is SSRF?
Server-side request forgery (SSRF) is a web security vulnerability that allows attackers to cause the server-side application to make requests to unintended locations. 

In typical SSRF attacks, the attacker might cause the server to make a connection to internal-only services within the organization's structure. In other cases, they might be able to force the server to connect to external systems. This can cause data leaks of sensitive information such as authorization credentials.

## SSRF Attacks Against the Server
In an SSRF attack, the attacker causes the server application to make an HTTP request back to the server that is hosting to application, via its loopback network interface. This typically involves supplying a URL with a hostname like `127.0.0.1` (reserver IP address that points to the loopback adapter) or `localhost` (common name for the adapter).

For example, imagine a shopping application that lets the users view if an item is in stock at a particular store. To provide the stock information, the application must query various back-end REST APIs. It does this by passing the URL to the relevant back-end API endpoint via a front-end HTTP request. When a user views the stock status for an item, their browser makes the following request:

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

This causes the server to make a request to the specific URL, retrieve the stock status, and return it to the user.

In this example, an attacker can modify the request to specify a URL local to the server:

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```

The server fetches the contents of the `/admin` URL and returns it to the user.

An attacker can visit the `/admin` URL, but the administrative functionality is only accessible to authenticated users. This means an attacker won't see anything of interest. However, if the request to `/admin` comes from the local machine, the normal access controls may be bypassed. The application grants full access to admin functionality, because the request looks to be from a trusted source.


Why do apps behave this way, and implicitly trust requests from the local machine? There are various reasons:

- The access control check might be implemented in a different component that sits in front of the application server. When a connection is made back to the server, the check is bypassed.
- For disaster recovery purposes, the app might allow admin access without logging in, to any user coming from the local mahcine. This provides ways for admins to recover the system if they lose their credentials. This assumes that only a fully trusted user would come directly from the server.
- The admin interface might listen on a different port number than the main application and might not be reachable directly by users.

These kind of trust relationships, where requests originating from the local machine are handled differently than ordinary requests, often lead SSRF to become a critical vulnerability. 

## SSRF Attacks Against other Back-end Systems
In some cases, the app server is able to interact with back-end systems that are not directly reachable by users. These systems often have non-routable private IP addresses. The back-end systems are normally protected by the network topology, so they may have a weaker security posture. In many cases, internal back-end systems contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems.

In the previous example, imagine there is an administrative interface at the back-end URL: `https://192.168.0.68/admin`. An attacker can submit the following request to exploit the SSRF vulnerability and access the interface:

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://192.168.0.68/admin
```