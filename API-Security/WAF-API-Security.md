## BigIP WAF API Security Deployment

#### This document explain how to deploy API Security using AWAF Feature

---

### Deploy WAF Policy
1. Download following file to your laptop

[Swagger JSON File](Swagger/F5EMEASSA-API-Sentence-2022-v1.yaml)
   
3. Click Access in "bigIP" node , choose TMUI
<img width="473" height="246" alt="Image" src="https://github.com/user-attachments/assets/1586301c-2719-4db6-9e9c-8dc628531092" />

4. Log In using
User:
```
admin
```
Password:
```
f5demo#1
```
<img width="666" height="422" alt="Image" src="https://github.com/user-attachments/assets/23f43b2a-b5f4-48bd-8d33-1164b280f8d0" />

4. Click Security -> Application Security -> Security Policies -> Policies List
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

6. Validate Positif Security (URL) has been added

<img width="736" height="786" alt="Image" src="https://github.com/user-attachments/assets/cf643e48-1e2d-4638-ad2a-e7b81058073c" />
<img width="1458" height="681" alt="Image" src="https://github.com/user-attachments/assets/91193fee-9d26-42e3-b03c-392d20c3f864" />

7. Assign WAF Policy to VS

Open VS List -> Click Security ->  Select Enabled in "Application Security Policy" -> Choose Policy "sentence-api-security-waf" -> Ennable log profile and add Log all request

<img width="1458" height="654" alt="Image" src="https://github.com/user-attachments/assets/ae952c0a-2cce-4981-8ee6-4a4dffe5e83f" />

<img width="1458" height="654" alt="Image" src="https://github.com/user-attachments/assets/d1ff73fc-a566-4cfd-a681-1a703457a231" />

<img width="1458" height="654" alt="Image" src="https://github.com/user-attachments/assets/15cdc940-018b-4acc-8ec8-4d68a2375f3b" />

---

### Test Waf Policy
1. Click Access in "Client" node , choose WebShell
<img width="506" height="536" alt="Image" src="https://github.com/user-attachments/assets/6ad544fd-2eba-4058-9ce9-6986bc572f1a" />

2. Log In using Ubuntu with this command
```
su - ubuntu
```
3. Try this API call , result should be good because API is valid
```
curl -H "Content-Type: application/json;charset=UTF-8" http://api.sentence.com/api/animals
```
4. Try unknown API call , result should be error because API is unknown (not included in swagger)
```
curl -H "Content-Type: application/json;charset=UTF-8" http://api.sentence.com/api/colors
```
<img width="1029" height="321" alt="Image" src="https://github.com/user-attachments/assets/2ed91777-8eb3-4d70-becf-0d1367b90a6f" />

5. Open event log in the big ip check error message. Error result should be illegal URL

<img width="580" height="457" alt="Image" src="https://github.com/user-attachments/assets/a2145788-ea1e-4987-8178-4034afec8919" />

<img width="1117" height="457" alt="Image" src="https://github.com/user-attachments/assets/e2780b41-33bf-4ac5-b3a2-0e852ce55056" />

6. Try an incorrect-schema API call — the request should fail because a required parameter is an integer but a string was provided.

```
curl --location --request DELETE 'http://api.sentence.com/api/adjectives/beautiful'
```
<img width="1117" height="68" alt="Image" src="https://github.com/user-attachments/assets/dbab9279-1004-4fd5-9450-3ccbf6972ee8" />

7. Open event log in the big ip check error message. Error result should be Expected data type inconsistent
<img width="1471" height="574" alt="Image" src="https://github.com/user-attachments/assets/a878c442-0748-41a6-a614-718eb67ff662" />

---
