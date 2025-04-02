# OS Command Injection
> Command injection vulnerabilities enable an attacker to execute arbitrary operating system (OS) commands on the server. This gives them full control over the server, compromising the application and all of its data.

## What is OS Command Injection?
OS command injection is also known as shell injection. It allows attackers to execute OS commands on the server that is running an app, and typically if this is available to an attacker, they can fully compromise the application and its data. Often attackers can leverage command injection to compromise the hosting infrastructure, and from there exploit trust relationships to further pivot their attack.

## Useful Commands
After identifying a OS command injection vulnerability, there are some commands that are useful for identifying system information.

| Purpose | Linux | Windows |
| --- | --- | --- |
| Current user | `whoami` | `whoami` |
| Operating System | `uname -a` | `ver` |
| Network Config | `ifconfig` or `ip a` | `ipconfig /all` |
| Network Connections | `netstat -an` | `netstat -an` |
| Running Processes | `ps aux` or `ps ef` | `tasklist` |

## Injecting OS Commands
In this example, a shop lets the user view if an item is in stock at a particular store. This is accessed via the following URL:

```http
https://insecure-website.com/stockStatus?productID=381&storeID=29
```

To provide the stock information, the app must query various legacy systems. The functionality is implemented by calling to a shell command with the product and store ID as parameters:

```bash
stockreport.pl 381 29
```

This command outputs the stock status, which is returned to the user.

The application implements no defenses so an attacker could inject the following input to execute a command of their choice:

```bash
& echo aiwefwlguh &
```

If this is submitted as the `productID` parameter, the following command is executed:

```bash
stockreport.pl & echo aiwefwlguh & 29
```

The `echo` command caused the given string to be echoed in the output. This is a good way to test for OS injections. The `&` is a shell command separator. In this example, it causes three command to execute so the output to the user would be:

```bash
Error - productID was not provided
aiwefwlguh
29: command not found
```

This shows that:
- The original `stockreport.pl` was executed without its parameters.
- The `echo` command was executed with the string `aiwefwlguh` as its parameter.
- The `29` was not a command, so the error `command not found` was returned.

Placing the additional `&` after the injected command is useful because it separates the injected command from whatever follows the injection point. This reduces the chance that what follows will prevent the injected command from executing.