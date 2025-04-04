# Path Traversal
> Path traversal (also known as directory traversal) vulnerabilities enable an attacker to interact with arbitrary files on the server, giving them access to sensitive data. If they can also write to these files, they can potentially modify application data or behavior, ultimately taking full control of the server.


## What is path traversal?
Also known as directory travesal, this vulnerablity allows attackers to read files on a server that is running a web application.

Such files can include:

- Application code/data
- Credentials for back-end systems
- Operating System files

In severe cases, attackers may also be able to write to arbitrary files, letting them modify application's behavior or data, which can result in full control of the server.


## Reading arbitrary files via path traversal
In the example given, a shopping application might display images of items for sale and might use the following HTML:

```
<img src="/loadImage?filename=218.png">
```

The `loadImage` URL takes a `filename` parameter and returns the contents of that file. Image files are stored on the disk in the location `/var/www/images`. To return an image, the application appends the requested filename to this base directory and uses an API to read the contents in the file, in the case of the example: `/var/www/images/218.png`.

This application implements no defenses against path traversal attacks, therefore an attacker can request the following URL to recieve `/etc/passwd`:

```
https://insecure-website.com/load`Image?filename=../../../etc/passwd
```

This causes the application to read the following path: `/var/www/images/../../../etc/passwd`.

The sequence `../` is valid in a file path as it means to step up one level in the directory tree. The three consecutive `../` steps up three levels in the directory tree to the filesystem root so the actual file that's read is `/etc/passwd`.

In standard UNIX systems, the `/etc/passwd` file contains a list of all users on the system, but other files can be accessed using the same method.

On Windows, both `../` and `../` are valid path separators, so the same attack can be performed so the followiung URL will also work on Windows:

```
https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini
```