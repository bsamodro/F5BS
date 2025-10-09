## BigIP WAF API Security Deployment

#### This document explain how to deploy API Security using AWAF Feature

---

### Deploy WAF Policy
1. Download following file to your laptop
   
2. Click Access in "bigIP" node , choose TMUI
<img width="473" height="246" alt="Image" src="https://github.com/user-attachments/assets/1586301c-2719-4db6-9e9c-8dc628531092" />

3. Log In using
User:
```
admin
```
Password:
```
f5demo#1
```
<img width="666" height="422" alt="Image" src="https://github.com/user-attachments/assets/23f43b2a-b5f4-48bd-8d33-1164b280f8d0" />

4. Click Application Security -> Security Policies -> Policies List
<img width="982" height="838" alt="Image" src="https://github.com/user-attachments/assets/64df681b-ff42-4b09-9815-eedd742b138c" />
Click Create
<img width="1467" height="244" alt="Image" src="https://github.com/user-attachments/assets/8dd05f02-508d-4e2f-b357-1890c5e475cc" />

5. Add detail (Click OK for any popup windows)

Policy Name :
```
sentence-api-security-waf
```
Policy Template (Dropdown menu) :
```
API Security
```
OpenAPI Swagger File :
Upload File : "choose your downoladed file from first step"

Click Save
<img width="1065" height="715" alt="Image" src="https://github.com/user-attachments/assets/ec080821-7d34-4f4f-a8d4-f7f3c7e8a5f0" />
<img width="1463" height="307" alt="Image" src="https://github.com/user-attachments/assets/f577ba3e-cb94-4fb4-ab4e-e9254cce052f" />
