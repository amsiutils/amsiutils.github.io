---
layout: post
title:  "Simple Active Directory Lab"
date:   2019-09-23 00:00:00 -0400
categories: 
---

This sets up AD, DHCP using only powershell with the following:

- Hostname: DC
- Domain: internal.local
- IP: 10.0.0.1
- DHCP: 10.0.0.2 to 250

#### Setup

[Download and install Windows Server][download].

#### Config Networking

```
Rename-computer -newname DC
$ipaddress = "10.0.0.1"
$dnsaddress = "127.0.0.1"
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress $ipaddress -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddresses $dnsaddress
Restart-Computer
```

#### Config Active Directory

```
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName internal.local
```

#### Config DHCP

```
Install-WindowsFeature -Name DHCP
Add-DhcpServerv4Scope -name "Scope" -StartRange 10.0.0.2 -EndRange 10.0.0.250 -SubnetMask 255.255.255.0 -State Active
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName dc.internal.local
Set-DhcpServerv4OptionValue -DnsDomain internal.local -DnsServer 10.0.0.1
Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
Set-DhcpServerv4DnsSetting -ComputerName "dc.internal.local" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
Add-DhcpServerInDC -DnsName dc.internal.local -IPAddress 10.0.0.1
Restart-service dhcpserver
```

#### Join Domain

From a Windows 7/10 host:

```
Rename-computer -newname PC1
Restart-Computer
add-computer –domainname internal.local -Credential internal.local\administrator -restart –force
```

[download]: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019?filetype=ISO
