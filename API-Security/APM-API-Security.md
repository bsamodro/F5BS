## BigIP APM API Security Deployment

#### This document explain how to deploy API Security using APM Protection Profile in APM

---

### Generate JWT Key and Token in ubuntu

in this lab, we will not generate the JWT on the BIG-IP. Instead, we will create the JWT key and token on Ubuntu, then export them to the BIG-IP to be used as the source for API Authorization Control.
1. Go to Client Node Webshell Tab and go to jwt-preparation directory (user is ubuntu)
```
cd jwt-preparation/
```
2. Verify 4 json file, 1 json file for header, 2 json file for different user payload, 1 json file for expiry token
```
for i in `ls *json`; do echo $i; cat $i; echo ;done
```
3. Generate base64 Key & copy the value to notepad, this will be used for export jwt in big ip
```
cat key.txt | base64 | tr '+/' '-_' | tr -d '='
```
```
ZjVkZW1vCg
```

---
### Deploy JWT Internal Authorizaion in BigIP
1. 

   
