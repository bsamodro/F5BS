## BigIP APM API Security Deployment - Part 2

#### This document explain how to deploy API Protection Profile and Assign it to VS

---

### Generate JWT Key and Token in ubuntu

in this lab, we will not generate the JWT on the BIG-IP. Instead, we will create the JWT key and token on Client Node, then export them to the BIG-IP to be used as the source for API Authorization Control.
1. Go to Client Node Webshell Tab and go to jwt-preparation directory (user is ubuntu)
```
cd jwt-preparation/
```
2. Verify 4 json file, 1 json file for header, 2 json file for different user payload, 1 json file for expiry token
