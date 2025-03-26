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
From a security perspective, one of the worst possible scenarios is if a website allows you to upload server-side scripts, such as PHP, Java, or Python files, and is configured to execute them. This makes it incredibly easy to create a web shell on the server.

```
Web shells are scripts that enable attackers to execute commands on a remote web server simply by sending HTTP requests to the right endpoint.
```

If you're able to sucessfully upload a web shell, you basically have full control over the server. This means you can read and write files, exfiltrate data, and use the server to pivot to internal infrastructure and external hosts. For example, the following PHP could be used to read arbitrary files:

```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

Once uploaded, sending a request for this malicous file will return the target' contents in the response.

A more versatile web shell may look like:

```php
<?php echo system($_GET['command']); ?>
```

This script enables you to pass a command via a query parameter such as:

```http
GET /example/exploit.php?command=id HTTP/1.1
```

## Exploiting Flawed Validation of File Uploads
In reality, it is unlikely to find a website with no protection against file upload attacks. However, it is still possible to find a site that has a flawed validation mechanism.

## Flawed File Type Validation
When submitting HTML forms, browsers normally send the given data in a `POST` request with the content type `application/x-www-form-url-encoded`. This is fine if one is sending simple data like a name or address. However, it is not suitable for sending binary data such as images or a PDF. For this, `multipart/form-data` is used.

Considering a form that contains fields for uploading an image, a description, and a username. The form might look like:

```http
POST /images HTTP/1.1
    Host: normal-website.com
    Content-Length: 12345
    Content-Type: multipart/form-data; boundary=---------------------------012345678901234567890123456

    ---------------------------012345678901234567890123456
    Content-Disposition: form-data; name="image"; filename="example.jpg"
    Content-Type: image/jpeg

    [...binary content of example.jpg...]

    ---------------------------012345678901234567890123456
    Content-Disposition: form-data; name="description"

    This is an interesting description of my image.

    ---------------------------012345678901234567890123456
    Content-Disposition: form-data; name="username"

    wiener
    ---------------------------012345678901234567890123456--
    ```

    The message body is split into seperate parts for each of the form's inputs. Each part contains a `Content-Disposition` header which provides information about the field it relates to. These individual parts may also contain their own `Content-Type` header, which tells the server the MIME type of the data that was submitted using this input.

    One way that websites may attempt to validate the uploads is to check that the input-specific `Content-Type` header matches the expected MIME type. If the server is only expecting images, it may only allow types such as `image/jpeg` or `image/png`. 

    Problems arise when the value of the header is implicitly trusted by the server. If no validation is performed, this defense can be easily bypassed.