# File Upload Vulnerabilities
> Any functionality that enables users to upload files to the server's filesystem are inherently dangerous. Failing to enforce proper restrictions on the files that users are allowed to upload can potentially enable an attacker to run arbitrary system commands, giving them full control over the server.

## What are File Upload Vulnerabilities?
File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like the name, type, content, or size. Failing to properly enforce restrictions on these could mean that even basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.

In some cases, the act of uploading the file is enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

## How do File Upload Vulnerabilities Arise?
It's rare for websites in the wild to have no restrictions whatsoever on which files users are allowed to upload. More commonly, developers implement what they believe to be robust validation that is either inherently flawed or can be easily bypassed.

For example, they may attempt to blacklist dangerous file types, but fail to account for parsing discrepancies when checking the file extension. As with any blacklist, it's also easy to accidentally omit the more obscure file types that may still be dangerous.

In other cases, a site may attempt to check the type by verifying properties that can be easily manipulated by an attacker.

Ultimately, even robust validation measures may applied inconsistently across a network of hosts and directories, resulting in discrepancies that can be exploited.

## Exploiting Unrestricted File Uploads to Deploy a Web Shell

## Exploiting Flawed Validation of File Uploads


## Flawed File Type Validation
