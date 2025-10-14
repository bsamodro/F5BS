## BigIP APM API Security Deployment - Part 1

#### This document explain how to prepare JWT before use it for API Validation in APM

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
echo -n f5demo | base64 | tr '+/' '-_' | tr -d '='
```
```
ZjVkZW1v
```

---
### Deploy JWT Internal Authorizaion in BigIP

1. Add New JWK :

a. Click Access -> Federation -> JSON Web Token -> Key Configuration
<img width="597" height="848" alt="Image" src="https://github.com/user-attachments/assets/5f654c1c-4c8b-4d90-8c18-48db505cbae5" />

b. Click Create
<img width="1484" height="226" alt="Image" src="https://github.com/user-attachments/assets/49689ecd-be3c-4ef0-9dce-5403af83a127" />

c. Fill the parameter using following value, and click save after finished

| Properties         | Value             | 
|--------------------|-------------------|
| Name               | f5demo-jwk-octet  |
| JWT Type           | JWS               |
| Type               | Octet             |
| Signing Algorithm  | HS256             |
| Encoding Format    | Base64url         |
| Shared Secret      | ZjVkZW1v          |

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
| Issuer             | f5demo.udf               |
| Access Token Expires In | 0                   |
| Sighning Algoritm  | Allowed : HS256          |
| JWK Allowed List   | Allowed : /Common/f5demo-jwk-octet   |

<img width="682" height="796" alt="Image" src="https://github.com/user-attachments/assets/fedd15e0-9b30-46ab-b3d8-eb881859a370" />
<img width="1455" height="192" alt="Image" src="https://github.com/user-attachments/assets/817fe409-fa5c-444f-b733-39add698c0af" />

3. Add New oAuth Server Provider List
   
a. Click Access -> Federation -> oAuth Client / Resource Server -> Provider
<img width="567" height="778" alt="Image" src="https://github.com/user-attachments/assets/286ef954-6624-4beb-8869-87376d33ade1" />

b. Click Create
<img width="1480" height="459" alt="Image" src="https://github.com/user-attachments/assets/fbba2575-a28c-4e4c-8ae9-a875a530043b" />

c. Fill the parameter using following value, unset "Use Auto JWT" check button 
<img width="1468" height="670" alt="Image" src="https://github.com/user-attachments/assets/fb5aec55-71b7-4afa-a509-bec1a491cb75" />

| Properties         | Value                    | 
|--------------------|--------------------------|
| Name               | f5demo-jwt-rs-provider   |
| Type               | Custom                   |

d. Choose  f5demo-jwt-token-config in JWT Dropbox and Click Save
<img width="1468" height="513" alt="Image" src="https://github.com/user-attachments/assets/3c17c590-7d92-43e8-8e25-eef0a2382e24" />
<img width="1468" height="450" alt="Image" src="https://github.com/user-attachments/assets/6adaad62-5e1f-4727-bf57-59ba5b2a27ef" />

4. Add New Provider List

a. Click Access -> Federation -> JSON Web Token -> Provider List -> Click Add
<img width="557" height="732" alt="Image" src="https://github.com/user-attachments/assets/d3711084-7dba-485a-885b-051b2be1caff" />

b. Choose provider name  f5demo-jwt-rs-provider -> Click Add -> Click Save
<img width="810" height="262" alt="Image" src="https://github.com/user-attachments/assets/c19163f6-b839-42e6-96d9-be0693fe1330" />
<img width="1457" height="194" alt="Image" src="https://github.com/user-attachments/assets/58547681-434e-4516-9384-6e74e4dbcb6b" />

---
[⬅️ Previous](WAF-API-Security.md) | [🏠 Home](readme.md) | [➡️ Next](APM-API-Security-Part2.md)
