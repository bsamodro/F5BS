<img width="805" height="717" alt="2_add_token_authentication_2" src="https://github.com/user-attachments/assets/9ba649a3-8630-4a66-ab32-e49a3a21d5f8" />## BigIP APM API Security Deployment - Part 3

#### This document explain about how to mitigate one of BOLA (Broken Level Object Authentication) Attack
---

### Bola Attack Mitigation sample configuration and testing

1.  Click Access -> API Protection -> Profile
<img width="976" height="796" alt="Image" src="https://github.com/user-attachments/assets/31743082-c016-4c74-bae6-95d845d95f75" />

2.  Click "Edit" in sentence-api-protection row
<img width="1450" height="201" alt="Image" src="https://github.com/user-attachments/assets/49076a73-0998-4d99-be6c-ad293fee9421" />

3.  Click "Plus Button" in fromt of GET /api/animals/{id}
<img width="832" height="604" alt="Image" src="https://github.com/user-attachments/assets/36c1ae72-0697-4ae1-836e-57c77e44a852" />

4. Select "General Purpose" tab -> select "Empty" radio button -> click "Add Item"
<img width="805" height="717" alt="Image" src="https://github.com/user-attachments/assets/01b5a515-539e-432f-92ab-9ee78ffeb1ab" />

5. Change name in Properties tab
   ```
   checkUserAnimal
   ```
<img width="625" height="732" alt="Image" src="https://github.com/user-attachments/assets/8f124a84-056c-4c00-8b51-90461728418c" />

6. Click Branch Rules tab -> Add Branch Rule -> update name parameter to "successful" -> Click "change" in Expression
<img width="625" height="732" alt="Image" src="https://github.com/user-attachments/assets/52c5e657-0bf8-43a9-83f7-6a618a28f45b" />

7. Type following text in Advanced box
```
expr {[regexp {^https?://[^/]+/api/animals/(\d+)/?} [mcget {perflow.category_lookup.result.url}] -> id] && ($id == [mcget {subsession.oauth.scope.last.jwt.animalID}])}
```
<img width="625" height="732" alt="Image" src="https://github.com/user-attachments/assets/431c982c-9eec-4e11-a4df-12fbf7ce9b41" />

Click "Save"

<img width="625" height="732" alt="Image" src="https://github.com/user-attachments/assets/9dfce6f2-18a1-487a-858a-fc489c769098" />

