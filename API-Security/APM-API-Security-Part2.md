## BigIP APM API Security Deployment - Part 2

#### This document explain how to deploy API Protection Profile ,  Assign it to VS, Test Expired JWT and Rate Limit per JWT User

---

### Deploy API Protection Profile

1. Download API Protection Profile
   Click Access -> Guided Configuration -> Wait until all configuration appear
<img width="1210" height="442" alt="Image" src="https://github.com/user-attachments/assets/da8ea788-4f69-43d1-87a5-acbc3ea5dea3" />

2.  Click Access -> API Protection -> Profile , Wait until profile list open then click Create
<img width="1709" height="685" alt="Image" src="https://github.com/user-attachments/assets/515da4b8-664b-41f7-ae74-6bac77d0e46e" />

3. Fill parameter as follow
   
| Parameter          | Value                                     | 
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

6. Edit Rate Limiting
Click "Rate Limiting" and Click "sentence-api-protection auto rate limiting_key1"
<img width="1433" height="723" alt="Image" src="https://github.com/user-attachments/assets/7d41690a-f445-4bfc-846c-90760368269c" />

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

1.  Click Access -> API Protection -> Profile
<img width="976" height="796" alt="Image" src="https://github.com/user-attachments/assets/31743082-c016-4c74-bae6-95d845d95f75" />

2.  Click sentence-api-protection
<img width="1457" height="224" alt="Image" src="https://github.com/user-attachments/assets/653e26bf-72eb-4121-b80a-b191c721eb1d" />

3.  In Rate Limiting Frame , click "Create"
<img width="1446" height="722" alt="Image" src="https://github.com/user-attachments/assets/55e44f18-c98d-4db2-b1ae-93f014d5afca" />

4.  Fill fillowing parameter, and click "Add"

| Parameter          | Value                             | 
|--------------------|-----------------------------------|
| Name               | api-ratelimit-rule 1              |
| Selected Keys      | UserID                            |
| Request Quota      | Check Enable , 3 rer per 2 min    |
| Spike arrest       | UnCheck Enable                    |
<img width="1446" height="396" alt="Image" src="https://github.com/user-attachments/assets/eef0cb45-a559-4d96-b0e0-69a3a821de9e" />

5. Click "Save"
<img width="1446" height="396" alt="Image" src="https://github.com/user-attachments/assets/68972b8d-b1a8-4752-bbb0-6a6ab6621c69" />

6. Click Access Control and select Edit
<img width="1446" height="396" alt="Image" src="https://github.com/user-attachments/assets/7e07ba06-f5ff-409f-9c47-f7547fb41f06" />

7. Click "plus" button in front of "Get /api/animals/" => we want to limit request api /api/animals per user for 3 request per minute
<img width="676" height="532" alt="Image" src="https://github.com/user-attachments/assets/31cb3ec5-a7bd-4fbe-9b3d-690953db6e2a" />

8. Select tab "Traffic Management" and tick "API Rate Limit" radio Button
<img width="816" height="721" alt="Image" src="https://github.com/user-attachments/assets/630f9c64-77fc-471a-95ee-1fba2c46a64c" />

9. Choose "rate limit" in drop box "response as follow and click new entry , choose rate limit rule that we create previously and click "save"
<img width="625" height="721" alt="Image" src="https://github.com/user-attachments/assets/49aea9e1-9164-46d4-928e-cae9a6d4fb46" />
<img width="816" height="563" alt="Image" src="https://github.com/user-attachments/assets/b9fcba56-b462-4735-8fd0-15043597c79f" />

---
#### Test Part 2 : Rate limit for user Token1

1. go to Client WebShell , login as ubuntu
2. Prepare token
```
cd /home/ubuntu/jwt-preparation
Token1=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_steve.json` 
Token1exp=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_steve_expired.json` 
Token2=`/home/ubuntu/jwt-preparation/makejwt.sh key.txt header.json payload_tony.json`
```
3. Run the following command four times to test the rate limit for token1. The fourth request should be rejected
```
# Test Token 1
curl -v -H "Content-Type: application/json;charset=UTF-8" -H "Authorization: Bearer $Token1"  http://api.sentence.com/api/animals
```
Run the following command once to test the API call using token2. The request should be accepted because the rate limit applies only to token1
```
# Test Token 2
curl -v -H "Content-Type: application/json;charset=UTF-8" -H "Authorization: Bearer $Token2"  http://api.sentence.com/api/animals

```
---
[⬅️ Previous](APM-API-Security-Part1.md) | [🏠 Home](readme.md) | [➡️ Next](APM-API-Security-Part3.md)
