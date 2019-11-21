---
layout: post
title:  "Deploy Winlogbeat Using Group Policy (GPO)"
date:   2019-11-22 00:00:00 -0400
categories: 
---

This guide will configure Winlogbeat to pipe sysmon and powershell loging to logstash, and deploy itself as a service for all endpoints.

It assumes that the previous [ELK / Elastic stack set up][elk-post] was installed and configured successfully and that [Sysmon][sysmon-post] and [PowerShell script logging][powershell-post] has been enabled via GPO on all endpoints.

#### Download

[Winlogbeat][winlogbeat-link] 

[Winglogbeat config yml file][winlogbeat-config]

[GPO Powershell Script][winlogbeat-ps]

Copy `logstash-forwarder.crt` from your ELK instance with [pscp][pscp-link].

```
pscp.exe <username>@<ELK IP>:/etc/pki/tls/certs/logstash-forwarder.crt .
```

#### Create winlogbeat folder in SYSVOL and copy files

```
C:\>dir /b \\dc.internal.local\sysvol\internal.local\winlogbeat
install.ps1
logstash-forwarder.crt
winlogbeat-7.4.2-windows-x86_64.zip
winlogbeat.yml
```

#### Configure

`winlogbeat.yml` has been already been configured to pipe sysmon and powershell logging to logstash.

Point `winlogbeat.yml` to your logstash host / IP

```
output.logstash:
  # The Logstash hosts
  hosts: ["<ELK IP>:5044"]
```

Modify `install.ps1` as required

```
$source = '\\dc.internal.local\sysvol\internal.local\winlogbeat\winlogbeat-7.4.2-windows-x86_64.zip'
$cert = '\\dc.internal.local\sysvol\internal.local\winlogbeat\logstash-forwarder.crt'
$config = '\\dc.internal.local\sysvol\internal.local\winlogbeat\winlogbeat.yml'
```

#### Create and link a Deploy Winlogbeat GPO

![GPO Deploy Winlogbeat](/assets/winlogbeat.png)

[winlogbeat-link]: https://www.elastic.co/downloads/beats/winlogbeat
[elk-post]: https://amsiutils.github.io/2019/11/18/quick-elastic-stack.html
[sysmon-post]: https://amsiutils.github.io/2019/11/21/deploy-sysmon-group-policy.html
[powershell-post]: https://amsiutils.github.io/2019/11/20/enable-powershell-logging-policy.html
[winlogbeat-config]: https://raw.githubusercontent.com/amsiutils/winlogbeat/master/winlogbeat.yml
[winlogbeat-ps]: https://raw.githubusercontent.com/amsiutils/winlogbeat/master/install.ps1
[pscp-link]: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
