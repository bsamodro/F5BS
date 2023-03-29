## BigIP AWAF - Custom Signature : Block Meta Character + remote execution command in URL Parameter

#### This Document will explain how to block URL which have metacharacter & remote execution command in parameter. BuiltIn Signature can only block remote execution command in Body (Payload)

#### Prerequisites : VS and Security Policy should have been configured. In this sample , I use "basic_Policy" for ASM security Policy
<img width="1000" alt="VSwithPolicy" src="https://user-images.githubusercontent.com/24970035/228450495-024cb807-1a21-4f3d-b40d-929f633619e0.png">

#### First : Check Signature Set in Learning and Blocking Setting
Click Security -> Application Security -> Policy Building -> Learning and Blocking Setting -> select basic_Policy in left top page.
### Please do not use all_signature in any Policy. If we add custom signature, any policy which use all_signature will be impacted

<img width="1000" alt="All SIignatures included" src="https://user-images.githubusercontent.com/24970035/228449952-ffd117bc-8dbf-40eb-aef3-3dff13d1f64d.png">

#### Second : Add Custom Signature to Attack Signature List
Click Security -> Options -> Application Security -> Attack Signature -> Attack Signature List -> Create
- Use "Other Application Activity" for Attack Type. Only All_signature is impacted for this type
- Use Regular expresstion to block the parameter. In this sample we want to block "=rm or |rm" in URL, sample : http://10.1.10.201/index.php/xxx=rm
- After we fill all mandatory field , click add
- We will add 2 sample attack signature

<img width="700" alt="custom_del" src="https://user-images.githubusercontent.com/24970035/228451408-79dd1c46-8b4c-4c46-ba43-6cf7b694260e.png">
<img width="700" alt="custom_rm" src="https://user-images.githubusercontent.com/24970035/228451435-3d6c08bb-bc97-4b6d-a5d9-7fccb7eafcee.png">

#### Third : Add new Attack Signature Sets
Click Security -> Options -> Application Security -> Attack Signature -> Attack Signature Sets -> Create
- Sample Name : Custom_Remote_Execution
- Use "Other Application Activity"
- Follow parameter in following picture

<img width="700" alt="Signatures Set 1" src="https://user-images.githubusercontent.com/24970035/228456532-188fce7c-7341-4d38-bf6d-e83bdad5e61c.png">
<img width="700" alt="Sgnatures Set 2" src="https://user-images.githubusercontent.com/24970035/228456557-41b2c9ac-aee3-4915-8744-9cfd27025290.png">

#### Forth : Assign new Signature Set to Learning and Blocking Setting - Attack Signatures
Click Security -> Application Security -> Policy Building -> Learning and Blocking Setting -> select basic_Policy in left top page.
- Explore "Attack Signatures"
- Click "Change"
- Tick Custom_Remote_Execution & Click "Change"
- Click "Save" and "Apply Policy"
<img width="1000" alt="Assign Set 1" src="https://user-images.githubusercontent.com/24970035/228468499-7ef00f97-75b0-4350-aad7-10b7ff096545.png">
<img width="790" alt="Assign Set 2" src="https://user-images.githubusercontent.com/24970035/228468531-38d4f268-1b46-44b1-8568-d2dcf359f8b8.png">
<img width="1000" alt="Assign Set 3" src="https://user-images.githubusercontent.com/24970035/228468548-1448a860-4a79-406f-9588-37894d978a47.png">

#### Signature has been built , its time to test
- On browser open  http://10.1.10.201/index.php/xxx=rm
<img width="918" alt="Test 1" src="https://user-images.githubusercontent.com/24970035/228470243-966f0a00-c013-4d74-85a6-82507514279c.png">
- On BigIP check request log : Security -> Event Logs -> Application -> Requests
<img width="1439" alt="Test 2" src="https://user-images.githubusercontent.com/24970035/228470259-7df9c362-5a99-48aa-80a6-aa74d9b373fd.png">

### ASM Has Built in tools to test Regex
- Click Security -> Options -> Application Security -> RegExp Validator
- Sample we want to test www.example.com/index.php.?12=rm using regex [=,|]rm

Please use this tool before we add regexp to custom signature


