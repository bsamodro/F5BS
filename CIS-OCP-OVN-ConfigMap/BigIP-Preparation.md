## CIS - OpenShift Workshop - BigIP Preparation Guide

#### This document explains how to prepare BigIP to integrate with OCP and deploy CIS in OCP Cluster

---

### AS3 Module Validation

Use the TMUI link of the both bigip node : bigip1 and bigip2
- login as admin -> click iApps -> click "Package Management LX" -> validate that f5-avvsvcs installed ( we use 3.54 version )

  <img width="1687" alt="Image" src="https://github.com/user-attachments/assets/e16b4f93-08af-4d7a-9076-f75a177df0b3" />
  
### Check HA Sync Type

HA Sync Type should be set to Manual to avoid conflict sync between CIS and TMOS HA. When we deploy CIS for BigIP CLuster HA, we will have 2 CIS per each node (standby and active) to do monitoring and config provisioning. If Sync Type is Auto, CIS will trigger issue that cis cannot update the config
- click "Device Management" -> click "Device Groups" -> Choose bigip-a_bigip-b_dg (Includes Self)


