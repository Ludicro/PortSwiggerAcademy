# Access Control

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