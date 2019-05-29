# VRRP-go
由golang实现的[VRRP-v3](https://tools.ietf.org/html/rfc5798), 点击超链接获取关于VRRP的信息。
[VRRP-v3](https://tools.ietf.org/html/rfc5798) implemented by golang，click hyperlink get details about VRRP

## example
```go
    package main
    
    import (
    	"VRRP/VRRP"
    	"net"
    	"fmt"
    	"time"
    )
    
    func main() {
    	var vr = VRRP.NewVirtualRouter(240, "ens33", false, VRRP.IPv6)
        	vr.SetPriorityAndMasterAdvInterval(243, time.Millisecond*700)
        	vr.SetAdvInterval(time.Millisecond * 700)
        	vr.SetPreemptMode(true)
        	vr.AddIPvXAddr(net.ParseIP("fe80::e7ec:1b6e:8e59:c96b"))
        	vr.AddIPvXAddr(net.ParseIP("fe80::e7ec:1b6e:8e59:c96a"))
        	vr.Enroll(VRRP.Backup2Master, func() {
        		fmt.Println("init to master")
        	})
        	vr.Enroll(VRRP.Master2Init, func() {
        		fmt.Println("master to init")
        	})
        	vr.Enroll(VRRP.Master2Backup, func() {
        		fmt.Println("master to backup")
        	})
        	go func() {
        		time.Sleep(time.Second * 120)
        		vr.Stop()
        	}()
        	vr.StartWithEventSelector()
    }
```

## To-DO

