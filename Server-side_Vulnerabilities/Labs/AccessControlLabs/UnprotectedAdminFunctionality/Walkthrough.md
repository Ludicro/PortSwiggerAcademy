## Access Control - Lab 1: Unprotected Admin Functionality

### Description

This lab has an unprotected admin panel.

Solve the lab by deleting the user `carlos`.

Starting web page:

![](home_page.png)

Since we have a page, and were given the information that there's an unsecured admin panel, the first place we could check is `robots.txt` to see if there are any pages that are hidden from web crawlers, but not from us.

Here we can see there is a page called `/administrator-panel`

![](robots.png)

If we navigate to that page we can see some admin functions to delete users and our target `carlos` is here.

![](admin_page.png)

Deleleting the user `carlos` will solve the lab.