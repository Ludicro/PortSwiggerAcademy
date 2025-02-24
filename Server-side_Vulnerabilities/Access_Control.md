# Access Control
> Access controls are designed to prevent users from interacting with data or functionality for which they don't have the relevant permissions. Due to the obvious security impact, broken access controls are critical bugs in their own right. They often also provide access to more attack surface, which could contain additional vulnerabilities.

## What is access control?

Access control is the application of constraints on who/what is authorized to perform actions or access to system resources. In the context of web apps, access control is dependent on authentication and session management.

- **Authentication** confirms the user is who they claim to be.
- **Session management** identifies the subsequent HTTP requests that are being made by the user.
- **Access control** determines whether the user is allowed to perform the requested action.

Broken access controls are common and often present a critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organizational, and legal contraints to a technical implementation. Access control design decisions have to be made by humans, which leads to a high potential for error.

## Vertical Privilege Escalation

If a user can gain access to functionality that they are not permitted to access, this is known as vertical privilege escalation.

For example, if a non-admin user can access an admin page where they can delete other user accounts, this is vertical privilege escalation.

## Unprotected Functionality
In it's simplest form, vertical privilege escalation occurs when applications do not enforce protection for sensitive functionality. An example of this is admin functionality is linked to an admin's welcome page, but not a user's  welcome page, but the user can access these functions by entering the admin URL. 

For example, a site might host sensitive functionality on the following URL:
```
https://insecure-website.com/admin
```
This might then be accessible to any user, not just admins. This URL might even be disclosed publicly such as on the `robots.txt` file:
```
https://insecure-website/robots.txt
```
Or the attacker can use a wordlist to brute force this location.

In some cases, sensitive information is in less predictable URLs. This is "security through obscurity". However, this is not a good pracice, nor does it provide good access control because users may discover the URL in a variety of ways.

For example, if an application hosts its admin functions at:
```
https://insecure-website.com/administrator-panel-yb556
```

This isn't the most predictable URL, but the application may still leak the URL to users. The URL might be disclosed in JavaScript that builds the user interface depending on the user's role:
```js
<script>
	var isAdmin = false;
	if (isAdmin) {
		...
		var adminPanelTag = document.createElement('a');
		adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
		adminPanelTag.innerText = 'Admin panel';
		...
	}
</script>
```
This script adds a link to the user's UI if they are an admin user, but this script is visible to all users.

## Parameter-based Access Control Methods
Some applications will determine a user's access right at login, and then store this information in a user-controllable location. This is normally in one of three ways:
- A hidden field
- A cookie
- A preset query string parameter

The application then makes access control decisions based on the value of this parameter. For example:
```
https://insecure-website.com/login/home.jsp?admin=true
https://insecure-website.com/login/home.jsp?role=1
```
This is insecure because a user can modify the value and access functionality they shouldn't have access to.

## Horizontal Privilege Escalation
Horizontal privilege escalation occurs if a user can gain access to resources that belong to another user, instead of their own. An example of this is if an employee can access the records of another employee, as well as their own.

These attacks may use similar exploit methods to vertical prrivilege escalation. For example, a user may access their own page using the URL:
```
https://insecure-website.com/myaccount?id=123
```
But if they modify the `id` parameter tobe `124`, they might gain access to another user's account, data, and functions. 

```
Note
This is an example of an insecure direct object reference (IDOR) vulnerability. This type of vulnerability arises where user-controller parameter values are used to access resources or functions directly.
```

In some apps, the parameter might not be a predictible value. Instead of an incrementing number, it might use globally unique identifiers (GUIDs) to prevent an attacker from guessing or predicting another user's ID. However, the GUIDs might be show elsewhere where other users are referenced, such as messages or reviews.

## Horizontal to Vertical Privilege Escalation
There are many cases where horizontal privilege escalation attacks can lead to vertical privilege escalation, by compromising more privileged users. For example, an attacker might be able to reset or capture a user's password. If they target an admin, they can then gain administrative access and perform vertical privilege escalation. 
They might be able to access another user's account using the method previously described: 
```
https://insecure-website.com/myaccount?id=456
```
If the target user is an application administrator, then the attacker will gain access to an administrative account page. This page might disclose the administrator's password or provide a means of changing it, or might provide direct access to privileged functionality.
