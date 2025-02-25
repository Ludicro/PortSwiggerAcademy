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
Usernames are generally easy to guess if they conform to a pattern, such as an email address. It is very common for business logins to be in the format of `firstname.lastname@company.com`. However, even if there is no obivious pattern, high priviledged accounts are often predictible such as `admin`, `root`, or `administrator`.

During auditing, check if the website discloses potential usernames publically. For example, are user profiles accessible without logging in? Even if the content is hidden, the name in the profile may still be the same as the username. Checking HTTP responses for email addresses is another good thing to check. Occasionally, responses contain email addresses for high priviledged users such as admins or IT.

## Brute-Forcing Passwords
Passwords can be similarly brute-forced, with the difficulty varying based on the strength of the password. Many websites adopt some form of password policy, which forces users to create high-entropy passwords that should be harder to crack using brute-force attacks.

These typically involve: 
- Minimum password length
- Mixture of lowercase and uppercase letters
- Mixture of numbers and symbols

However, while high-entropy passwords are more difficult for computers to crack, knowledge of human behavior can be used to exploit vulnerabilities that users introduce into a system. Rather than creating a strong password with a random combination of characters, users often make passowrds they can remember and try to modify them to fit policy. If `mypassword` isn't allowed, something like `Mypassword!` or `Myp4$$w0rd` may be used.

In cases where users are required to change their passwords regularly, it is also common for them to make minor changes. For example `Mypassword1!` may be changed to `Mypassword1?` or `Mypassword2!`.

Knowing likely credentials and predictable password patterns means that brute-force attacks can be sophisitcated and therefore effective.

## Username Enumeration
Username enumeration is when an attacker is able to observer changes in the website's behavior in order to validate if a given username is valid.

Username enumeration is typically done on login pages, such as when you enter a valid username but a bad password, or on forms where you enter a username that is already taken. This reduces the time and effort required to brute-force a login because the attacker is able to generate lists of valid users.

## Bypassing Two-Factor Authentication
At times, two-factor authentication is flawed when implemented, so it can be entirely bypassed.

If the user is first prompted for a password, then prompted for a code on a seperate page, the user is essentially in a "logged-in" state before they entered the verification code. In this case, it is worth testing if you can skip to the "logged-in only" pages after completeing the first authentication step. 

On occasion, you may find a site that doesn't check if you completed the second step before loading the page.