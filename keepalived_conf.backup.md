```bash
vrrp_script chk_haproxy {
    script "killall -0 haproxy"
    interval 2
    weight 2
}


vrrp_instance VI_1 {
    interface ens33
    state BACKUP
    priority 100
    virtual_router_id 51



    virtual_ipaddress {
        192.168.1.19
    }

 
    track_script {
         chk_haproxy
        }
}
```
