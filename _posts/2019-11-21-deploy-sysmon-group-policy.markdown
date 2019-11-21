---
layout: post
title:  "Deploy Sysmon Logging Using Group Policy (GPO)"
date:   2019-11-21 00:00:00 -0400
categories: 
---

#### Download

[Sysmon][sysmon-link] from [Windows Sysinternals][sysinternals-link]

[Sysmon config xml file][sysmon-config] (credits to [@SwiftOnSecurity][swiftonsecurity])

[Sysmon batch file][sysmon-batch]


#### Configure

Edit sysmon.bat and modify as required

```
SET DC=dc.internal.local
SET FQDN=internal.local
SET SYSMONCONFIG=%SYSMONDIR%\sysmonconfig-export.xml
SET GLBSYSMONCONFIG=\\%DC%\sysvol\%FQDN%\sysmon\sysmonconfig-export.xml
```

#### Create sysmon folder in SYSVOL and copy files

```
C:\>dir /b \\dc.internal.local\sysvol\internal.local\sysmon
sysmon.bat
Sysmon.exe
Sysmon64.exe
sysmonconfig-export.xml
```

#### Create and link a Deploy Sysmon GPO

![GPO Deploy Sysmon](/assets/sysmon.png)


[sysmon-link]: https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon
[sysinternals-link]: https://docs.microsoft.com/en-us/sysinternals/
[sysmon-config]: https://raw.githubusercontent.com/amsiutils/sysmon-config/master/sysmonconfig-export.xml
[swiftonsecurity]: https://github.com/SwiftOnSecurity/sysmon-config
[sysmon-batch]: https://raw.githubusercontent.com/amsiutils/sysmon-config/master/sysmon.bat