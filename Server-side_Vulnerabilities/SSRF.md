# Server-Side Request Forgery (SSRF)
> SSRF vulnerabilities enable an attacker to trigger malicious server-to-server requests to unintended URLs. As the server issuing the request is likely to have a strong trust relationship with other systems on the network, the attacker can potentially abuse this behavior to access data, functionality, and services that are not meant to be exposed to external users.

## What is SSRF?
Server-side request forgery (SSRF) is a web security vulnerability that allows attackers to cause the server-side application to make requests to unintended locations. 

In typical SSRF attacks, the attacker might cause the server to make a connection to internal-only services within the organization's structure. In other cases, they might be able to force the server to connect to external systems. This can cause data leaks of sensitive information such as authorization credentials.

## SSRF Attacks Against the Server


## SSRF Attacks Against other Back-end Systems
