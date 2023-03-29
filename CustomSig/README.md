## BigIP AWAF - Custom Signature : Block Meta Character + remote execution command in URL Parameter

### This Document will explain how to block URL which have metacharacter & remote execution command in parameter. BuiltIn Signature can only block remote execution command in Body (Payload)

### Prerequisites : VS and Security Policy should have been configured. In this sample , I use "basic_Policy" for ASM security Policy
<img width="1000" alt="VSwithPolicy" src="https://user-images.githubusercontent.com/24970035/228450495-024cb807-1a21-4f3d-b40d-929f633619e0.png">

#### First : Check Signature Set in Learning and Blocking Setting
Click Security -> Application Security -> Policy Building -> Learning and Blocking Setting -> select basic_Policy in left top page.
Please do not use all_signature in any Policy. If we add custom signature, any policy which use all_signature will be impacted

<img width="1000" alt="All SIignatures included" src="https://user-images.githubusercontent.com/24970035/228449952-ffd117bc-8dbf-40eb-aef3-3dff13d1f64d.png">

#### Second : Add Custom Signature to Attack Signature List
Click Security -> Options -> Application Security -> Attack Signature -> Attack Signature List -> Create
- Use "Other Application Activity" for Attack Type. Only All_signature is impacted for this type
- Use Regular expresstion to block the parameter. In this sample we want to block "=rm or |rm" in URL, sample : www.exmaple.com/index.php?a=rm
- After we fill all mandatory field , click add
- We will add 2 sample attack signature

![custom_rm](https://user-images.githubusercontent.com/24970035/228018007-c3b7671e-eb30-4b62-aed1-5a5508463f03.jpg)
