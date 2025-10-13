## BigIP APM API Security Deployment

#### This document explain how to deploy API Security using APM Protection Profile in APM

---

### Generate JWT Key and Token in ubuntu

in this lab, we will not generate the JWT on the BIG-IP. Instead, we will create the JWT key and token on Ubuntu, then export them to the BIG-IP to be used as the source for API Authorization Control.

---
### Deploy JWT Internal Authorizaion in BigIP
1. Download following file to your laptop

[Swagger JSON File](Swagger/F5EMEASSA-API-Sentence-2022-v1.yaml)
   
