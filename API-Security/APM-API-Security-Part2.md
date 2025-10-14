## BigIP APM API Security Deployment - Part 2

#### This document explain how to deploy API Protection Profile and Assign it to VS

---

### Deploy API Protection Profile

1. Download API Protction Profile
   Click Access -> Guided Configuration -> Wait until all configuration appear
<img width="1210" height="442" alt="Image" src="https://github.com/user-attachments/assets/da8ea788-4f69-43d1-87a5-acbc3ea5dea3" />

2.  Click Access -> API Protection -> Profile , Wait until profile list open then click Create
<img width="1709" height="685" alt="Image" src="https://github.com/user-attachments/assets/515da4b8-664b-41f7-ae74-6bac77d0e46e" />

3. Fill parameter as follow
   
| Properties         | Value                                     | 
|--------------------|-------------------------------------------|
| Name               | sentence-api-protection                   |
| Open API File      | Choose File JSON that has been downloaded |
| Authentication     | OAuth 2.0 -> Add                          |

Click Save
<img width="1226" height="430" alt="Image" src="https://github.com/user-attachments/assets/31b5cac4-a9ea-45af-9423-a2f63686b917" />

4. View path URL has been loaded
<img width="1463" height="678" alt="Image" src="https://github.com/user-attachments/assets/5dede798-87a7-41a2-ba38-eb3dcf0d46f8" />

5. Edit Default Response
Click Response and Click "sentence-api-protection auto response1"
<img width="1463" height="678" alt="Image" src="https://github.com/user-attachments/assets/e3aecdab-22de-4d02-a386-040e14669e10" />

Type "API Access Error" or Another Error Message in Body
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/bdf1d5bf-25de-4856-8811-d77ed2006812" />

Click Edit and Save
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/ad606f16-cbd6-48a8-881a-5a3a5a71d9e3" />

5. Edit Response 2
Click Response and Click "sentence-api-protection auto response2"
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/cf1a470f-bb2c-44d8-bb61-15d876a90ab9" />

Type "API Auth Error" or Another Error Message in Body
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/f7e61d5e-de81-4256-8bf8-8484689013c9" />

Click Edit and Save
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/7363e56e-f348-47e3-ac23-02b430f31379" />

6. Edit Response Rate Limiting
Click Response and Click "sentence-api-protection auto response api rate limiting3"
<img width="1470" height="625" alt="Image" src="https://github.com/user-attachments/assets/9572fb47-78dc-43cd-aa59-4a61d33445c6" />

Change Key Value to 
```
%{subsession.oauth.scope.last.jwt.sub}
```
We Plan to test rate limit per User
<img width="1463" height="629" alt="Image" src="https://github.com/user-attachments/assets/a066df67-0883-4e8f-b810-dec3b843c4c2" />

Click Edit and Save
<img width="1463" height="777" alt="Image" src="https://github.com/user-attachments/assets/0ac42d14-f59e-49d4-90fc-bf3d19fe1ffa" />

7. Edit Per Request Policy
Click Access Control and Per Request Policy - Edit
<img width="1463" height="722" alt="Image" src="https://github.com/user-attachments/assets/cc209bf4-8380-4373-8142-968159e7bbb8" />
Update Oauth Scope
<img width="945" height="826" alt="Image" src="https://github.com/user-attachments/assets/f1184a20-6843-4d78-8cda-e7e6f6a10993" />
refer to following UI 
<img width="1329" height="826" alt="Image" src="https://github.com/user-attachments/assets/98d6f15e-1e4d-4376-b617-f8b92d77d30b" />

8. Assign New API Protection Profile to VS
Click Virtual Server List under Local Traffic Menu, choose sentence-vs
<img width="1442" height="826" alt="Image" src="https://github.com/user-attachments/assets/2a0bc9f4-cb6d-4202-a0b3-6cbb01df9c52" />


Change API Protection Profile and Update
<img width="1217" height="826" alt="Image" src="https://github.com/user-attachments/assets/4c8fce41-fda1-49d7-adef-6c7e3a52d99f" />


9. Update VS Security policies , change Application Security Policy to sentence-api-protection
<img width="1217" height="826" alt="Image" src="https://github.com/user-attachments/assets/b5095192-03b7-4a05-ba78-6e4b5d6887e3" />
<img width="1217" height="826" alt="Image" src="https://github.com/user-attachments/assets/25640bc5-1ffd-48d1-bed7-8b843d981c7a" />

#### Test Part 1 : No Token, Good Token and Exired Token

1. go to Client WebShell , login as ubuntu
2. Prepare token
```
cd /home/ubuntu/jwt-preparation
Token1=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_steve.json` 
Token1exp=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_steve_expired.json` 
Token2=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_tony.json`
```
3. run following command to test API, review the result (no Token or Expired Token will have error response)
```
# Test no Token
curl -v -H "Content-Type: application/json;charset=UTF-8"  http://api.sentence.com/api/animals

# Test Token 1
curl -v -H "Content-Type: application/json;charset=UTF-8" -H "Authorization: Bearer $Token1"  http://api.sentence.com/api/animals

# Test Token 2
curl -v -H "Content-Type: application/json;charset=UTF-8" -H "Authorization: Bearer $Token2"  http://api.sentence.com/api/animals

# Test Token 1 Expired
curl -v -H "Content-Type: application/json;charset=UTF-8" -H "Authorization: Bearer $Token1exp"  http://api.sentence.com/api/animals
```

---
### API Rate Linit Configuration & Testing


