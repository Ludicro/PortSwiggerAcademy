## SSRF - Lab 2: Basic SSRF Against Another Back-End System

### Description
This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal `192.168.0.X` range for an admin interface on port `8080`, then use it to delete the user `carlos`.

We can approach this similar to the last lab, only this time, we know the request will have to be `http://192.168.0.X:8080/admin`. Now there's a lot of possible IPs it could be stored on, so I send the request to the Intruder a number payload ranging from 1-255 for the last octet. 

While it was running, we can see that `.10` has a status code of `200`, which is promising.

Since we already know the request from the last lab, now we can enter the following into the `stockAPI`: `http://192.168.0.10:8080/admin/delete?username=carlos`