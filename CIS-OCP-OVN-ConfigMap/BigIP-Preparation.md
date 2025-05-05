## CIS - OpenShift Workshop - BigIP Preparation Guide

#### This document explains how to prepare BigIP to integrate with OCP and deploy CIS in OCP Cluster

---

### AS3 Module Validation

Use the TMUI link of the both bigip node : bigip1 and bigip2
- login as admin -> click iApps -> click "Package Management LX" -> validate that f5-avvsvcs installed ( we use 3.54 version )

<img width="1687" alt="Image" src="https://github.com/user-attachments/assets/e16b4f93-08af-4d7a-9076-f75a177df0b3" />
  
### Check HA Sync Type (Should be Manual)

HA Sync Type should be set to Manual to avoid conflict sync between CIS and TMOS HA. When we deploy CIS for BigIP CLuster HA, we will have 2 CIS per each node (standby and active) to do monitoring and config provisioning. If Sync Type is Auto, CIS will trigger issue that cis cannot update the config

<img width="300" alt="Image" src="https://github.com/user-attachments/assets/8fee055b-f3bf-48fd-b42a-89330783c8a7" />

- click "Device Management" -> click "Device Groups" -> Choose bigip-a_bigip-b_dg (Includes Self)

 <img width="1672" alt="Image" src="https://github.com/user-attachments/assets/430144a8-f271-47a4-b272-b893271c90e6" />
 <img width="1672" alt="Image" src="https://github.com/user-attachments/assets/54266b8e-74fc-46b7-8d6c-e869e9ed0e80" />
