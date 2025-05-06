## CIS - OpenShift Workshop - Test and Validate Arcadia Application

#### This document explains how to Test Arcadia Application that has beed deployed using CIS

---

### BigIP Validation 

Use the TMUI link of the both bigip node : bigip1 and bigip2

- login as admin -> click "Local Traffic"  -> clicl "Virtual Servers" -> Change Partition to "arcadia-tenant"
  arcadia-VS should be Green

  
  
- Check Network Map by click  "Local Traffic" -> click "Network Map" -> New Tab will be open
  We can check VS overview, pool, ssl client, waf policy , etc
