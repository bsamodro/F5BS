## BigIP AWAF - Custom Signature : Block Meta Character + remote execution command in URL Parameter

### This Document will explain how to block URL which have metacharacter & remote execution command in parameter. BuiltIn Signature can only block remote execution command in Body (Payload)

### Prerequisites : VS and Security Policy should have been configured. In this sample , I use "basic_Policy" for ASM security Policy

#### First : Associate Signature Set in Learning and Blocking Setting
Click Security -> Application Security -> Policy Building -> Learning and Blocking Setting -> select basic_Policy in left top page.
Please do not use all_signature in any Policy. If we add custom signature, any policy which use all_signature will be impacted

![Custom_Sig](https://user-images.githubusercontent.com/24970035/227852373-d22fe6f7-3176-4d76-83e4-2ec9600bdfa8.jpg)

#### Second : Add Custom Signature to Attack Signature List
Click Security -> Options -> Application Security -> Attack Signature -> Attack Signature List -> Create
- Use "Other Application Activity" for Attack Type. Only All_signature is impacted for this type
- Use Regular expresstion to block the parameter. In this sample we want to block "=rm or |rm" in URL, sample : www.exmaple.com/index.php?a=rm
- After we fill all mandatory field , click add
- We will add 2 sample attack signature

