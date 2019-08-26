---
layout: post
title:  "Auditing Exchange Server"
date:   2019-08-26 00:00:00 -0400
categories: 
---

#### Mailbox Audit Logging (MAL)

Mailbox Audit Logging (MAL) is not enabled by default. Mailboxes accessed by non-mailbox owners (read, send) are not logged. Ascertain if mailbox auditing is enabled for users.

```
Get-Mailbox | Select Name,AuditEnabled
```

#### Run a non-owner mailbox access report

If MAL is enabled, verify that logs are detailed without errors in Exchange Admin Center (EAC).

```
Login "Exchange Admin Center" as exchange administrator.
- (left menu) compliance management
- (top menu) auditing
 - Run a non-owner mailbox access report.
```

[Run a non-owner mailbox access report][non-owner-report]

#### Mailbox Delegation

Mailboxes may be delegated to unauthorized parties, who may be able to send email on behalf of the user. Generate list of mailboxes that have been delegated to non-owners. Perform a reasonableness check that these mailboxes require delegation. E.g. CEO mailbox could be delegated to PA, but not to an Exchange Administrator. (CSV Output: “Name” = PA and “SendOnBehalfOf” = CEO)

```
Get-Mailbox | where {$_.GrantSendOnBehalfTo -ne $null} | select Name,Alias,UserPrincipalName,PrimarySmtpAddress,@{l='SendOnBehalfOf';e={$_.GrantSendOnBehalfTo -join ";"}} 
```

#### Admin Audit Log

Unauthorized Administrator activities may not be logged. Ascertain that Admin Audit Log is enabled and logging all cmdlets.

```
Get-AdminAuditLogConfig
```

- AdminAuditLogEnabled should be True
- AdminAuditLogCmdlets should be {*}
- AdminAuditLogParameters should be {*}
- AdminAudtLogAgeLimit should be 90

#### Exchange Administrator Activities

Exchange administrators may have grant themselves full access to user mailboxes. Review and enquire for any abnormal cmdlet used by the administrators to gain access to other mailboxes. E.g.

- Add-MailboxPermission
- Remove-Mailboxpermission
- Export-Mailbox

```
Search-AdminAuditLog | Select RunspaceID,CmdletName,RunDate,Succeeded
```

#### Impersonation of Mailboxes (Send As)

Exchange administrators may have previously granted permissions to themselves to send unauthorized email on behalf of others accounts. Verify that all exchange administrators are not able to impersonate sending emails on behalf of other accounts.

```
Get-User <ADID> | Get-ADPermission | where {$_.ExtendedRights -like 'Send*'} | Format-Table -Auto User,Deny,ExtendedRights
```

#### Full Access to Mailboxes

Exchange administrators may previously granted full access to other mailboxes, and could read emails. Verify that all exchange administrators do not have full access to any other mailboxes.

```
Get-Mailbox | Get-MailboxPermission -user <ADID> | where {$_.user.tostring() -ne "NT AUTHORITY\SELF" -and $_.IsInherited -eq $false} | Select identity,user
```








[non-owner-report]: https://docs.microsoft.com/en-us/exchange/security-and-compliance/exchange-auditing-reports/non-owner-mailbox-access-report
