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

Type "API Access Error" or Another Error Message in Body

Click Edit and Save

5. Edit Response 2
Click Response and Click "sentence-api-protection auto response2"

Type "API Auth Error" or Another Error Message in Body

Click Edit and Save

6. Edit Response 2
Click Response and Click "sentence-api-protection auto response api rate limiting3"

Change Key Value to 
```
%{subsession.oauth.scope.last.jwt.sub}
```
We Plan to test rate limit per User

Click Edit and Save



