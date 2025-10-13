## BigIP APM API Security Deployment

#### This document explain how to deploy API Security using APM Protection Profile in APM

---

### Generate JWT Key and Token in ubuntu

in this lab, we will not generate the JWT on the BIG-IP. Instead, we will create the JWT key and token on Client Node, then export them to the BIG-IP to be used as the source for API Authorization Control.
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

1. Add New JWK :

a. Click Access -> Federation -> JSON Web Token -> Key Configuration
<img width="597" height="848" alt="Image" src="https://github.com/user-attachments/assets/5f654c1c-4c8b-4d90-8c18-48db505cbae5" />

b. Click New
<img width="1484" height="226" alt="Image" src="https://github.com/user-attachments/assets/49689ecd-be3c-4ef0-9dce-5403af83a127" />

c. Fill the parameter using following value, and click save after finished

| Properties         | Value             | 
|--------------------|-------------------|
| Name               | f5demo-jwk-octet  |
| JWT Type           | JWS               |
| Type               | Octet             |
| Signing Algorithm  | HS256             |
| Encoding Format    | Base64url         |
| Shared Secret      | ZjVkZW1vCg        |

<img width="701" height="420" alt="Image" src="https://github.com/user-attachments/assets/5da9859b-2946-487b-81c9-12f21182d39a" />
<img width="1484" height="226" alt="Image" src="https://github.com/user-attachments/assets/93347adc-9920-424c-853c-9451d2add204" />

2. Add New JWT

a. Click Access -> Federation -> JSON Web Token -> Token Configuration
<img width="568" height="740" alt="Image" src="https://github.com/user-attachments/assets/6ece5cc2-4362-4b1c-b32c-d28790069803" />

b. Click Create
<img width="1466" height="189" alt="Image" src="https://github.com/user-attachments/assets/062bf4f3-14c3-42ff-bd44-be6d9e6a3ab1" />

c. Fill the parameter using following value, and click save after finished

| Properties         | Value                    | 
|--------------------|--------------------------|
| Name               | f5demo-jwt-token-config  |
| Issuer             | f5demo.net               |
| Access Token Expires In | 0                   |
| Sighning Algoritm  | Allowed : HS256          |
| JWK Allowed List   | Allowed : /Common/f5demo-jwk-octet   |

<img width="682" height="796" alt="Image" src="https://github.com/user-attachments/assets/fedd15e0-9b30-46ab-b3d8-eb881859a370" />
<img width="1455" height="192" alt="Image" src="https://github.com/user-attachments/assets/817fe409-fa5c-444f-b733-39add698c0af" />




   
