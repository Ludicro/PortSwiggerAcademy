# Authentication
> Authentication is the process of checking that a user really is who they claim to be. For example, a login form where you enter your username and password is one form of authentication mechanism. Flawed authentication mechanisms may allow an attacker to guess valid sets of credentials by automating thousands of login attempts using specialist tools like Burp Intruder.

## Authentication Vulnerabilities
Conceptually, authentication vulnerabilites are easy. However, due to the relationship between authentication and security, they are critical.

Authentication vulnerabilities allow attackers to gain access to sensitive data and functionality. They alsoexpose additional attack vectors. Because of this, it's important to learn how to identify and exploit authentication vulnerabilities, and how to bypass common protection measures.

## What is the Difference between Authentication and Authorization?
Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something. 

For example, authentication determines whether someone attempting to access a website with the username `Carlos123` really is the same person who created the account.

Once `Carlos123` is authenticated, their permissions determine what they are authorized to do. For example, they may be authorized to access personal information about others, or performing actions.

## Brute-Force Attacks
A brute-force attack is when an attacker uses a system of trial and error to guess valid user credentials. These attacks are typically automated using wordlists of usernames and passwords. Automating this process, especially using dedicated tools, potentially enables an attacker to make large amounts of login attempts very quickly.

Brute-forcing is not always a case of making completely random gusses at usernames and passwords. By applying basic logic and publically available knowledge, attackers can fine-tune their attacks to make more educated guesses. This increases the efficency of the attack tremendously. Wbesites that rely solely on password logins to authenticate their users are particularly vulnerable to brute-force attacks if proper protection isn't applied. 

## Brute-Forcing Usernames

## Brute-Forcing Passwords

## Username Enumeration

## Bypassing Two-Factor Authentication