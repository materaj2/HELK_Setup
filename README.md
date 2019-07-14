# HELK_Setup
HELK and friends
  
## Step  
1. Install HELK (https://github.com/Cyb3rWard0g/HELK)(Defaul user: helk and password: hunting)  
git clone https://github.com/Cyb3rWard0g/HELK.git  
cd HELK/docker
sudo ./helk_install.sh  
  
2. Download Sysmon from Microsoft  
- https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon  
  
3. Install Sysmon service(With Administrator privilege)  
sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n  

4. Download Sysmon configuration from SwiftOnSecurity  
- https://github.com/SwiftOnSecurity/sysmon-config  
- https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml  

5. Check current rules of sysmon  
sysmon.exe -c  

6. Update sysmon configuration
Sysmon.exe -c sysmonconfig-export.xml  
  
7. Download winlogbeat for sending log  
https://www.elastic.co/downloads/beats/winlogbeat  
  
8. Extract winlogbeat zip file and move to C:\Program Files\  
  
9. Move to winlogbeat folder  
cd 'C:\Program Files\winlogbeat-7.2.0-windows-x86_64\winlogbeat-7.2.0-windows-x86_64'  
  
10.Install winlogbeat service(With Administrator privilege)  
.\install-service-winlogbeat.ps1  
*** If find problem about "Cannot be loaded because the execution of scripts is disabled on this system.", please run 'set-executionpolicy remotesigned'  
  
In Winlogbeat.yml in "C:\Program Files\winlogbeat-7.2.0-windows-x86_64\winlogbeat-7.2.0-windows-x86_64"  
##### Normally winlogbeat.event_logs:  
```
winlogbeat.event_logs:
  - name: Application
    ignore_older: 72h

  - name: System

  - name: Security
    processors:
      - script:
          lang: javascript
          id: security
          file: ${path.home}/module/security/config/winlogbeat-security.js

  - name: Microsoft-Windows-Sysmon/Operational
    processors:
      - script:
          lang: javascript
          id: sysmon
          file: ${path.home}/module/sysmon/config/winlogbeat-sysmon.js
```
##### Comment host field in output.elastic:  
```
#output.elasticsearch:  
  # Array of hosts to connect to.  
  #hosts: ["localhost:9200"]  
```
##### Remove comment at output.logstash:  
```
#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts ### Specific IP here
  hosts: ["35.247.163.182:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]

  # Certificate for SSL client authentication
  #ssl.certificate: "/etc/pki/client/cert.pem"

  # Client Certificate Key
  #ssl.key: "/etc/pki/client/cert.key"
```  
  
## Source::
https://cyberwardog.blogspot.com/2017/02/setting-up-pentesting-i-mean-threat_87.html  

  
  
  
  
  
  
