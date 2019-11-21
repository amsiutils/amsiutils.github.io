---
layout: post
title:  "Deploy Winlogbeat Using Group Policy (GPO)"
date:   2019-11-21 00:00:00 -0400
categories: 
---

This guide assumes that the previous [ELK / Elastic stack set up][elk-post] was installed and configured successfully and that [Sysmon][sysmon-post] and [PowerShell script logging][powershell-post] has been enabled via GPO on all endpoints.

#### Download

[Winlogbeat][winlogbeat-link] 

[Winglogbeat config yml file][winlogbeat-config]

[GPO Powershell Script][winlogbeat-ps]


#### Configure

Edit sysmon.bat and modify as required

```
SET DC=dc.internal.local
SET FQDN=internal.local
SET SYSMONCONFIG=%SYSMONDIR%\sysmonconfig-export.xml
SET GLBSYSMONCONFIG=\\%DC%\sysvol\%FQDN%\sysmon\sysmonconfig-export.xml
```

#### Copy files to sysmon folder in SYSVOL

```
C:\>dir /b \\dc.internal.local\sysvol\internal.local\sysmon
sysmon.bat
Sysmon.exe
Sysmon64.exe
sysmonconfig-export.xml
```

#### Create and link a Deploy Sysmon GPO

![GPO Deploy Sysmon](/assets/sysmon.png)

[winlogbeat-link]: https://www.elastic.co/downloads/beats/winlogbeat
[elk-post]: https://amsiutils.github.io/2019/11/18/quick-elastic-stack.html
[sysmon-post]: https://amsiutils.github.io/2019/11/21/deploy-sysmon-group-policy.html
[powershell-post]: https://amsiutils.github.io/2019/11/20/enable-powershell-logging-policy.html
[winlogbeat-config]: 
[winlogbeat-ps]: 
