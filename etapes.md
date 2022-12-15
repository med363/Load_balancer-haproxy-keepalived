1. cree quatre box deux haproxy contient le package keepalived(master/backup) pour permet le haut disponibilite si l'une de haproxy tombe l'autre prend le releve
2. cree l'@ virtuel que l'utilise se connecte avec ((/etc/keepalived/keepalived.conf))
--------------------------
#definnie le script pour verifier que le haproxy rester s'executer
vrrp_script chk_haproxy {
    script "killall -0 haproxy"
    interval 2
    weight 2
}

# configuration de l'interface virtuelle
vrrp_instance VI_1 {
    #interface wan
    interface enp0s9
    #ce haproxy est doit etre le srv master et l'autre haproxy de backup
    state MASTER
    #ce haproxy doit avoir la priority la plus grd pour que soit le backup
    priority 500
    virtual_router_id 51


    #virtual ip adress doit etre dans le meme plage d'adress de deux haproxy
    virtual_ipaddress{
        192.168.1.X
    }

    #
    track_script{
         chk_haproxy
        }
}
-----------------------------
3. confire haproxy doit contenir l'@ du svrs balyer((etc/haproxy/haproxy.cfg))
-----------------------------
     # Add to the end
     # Define frontend
     frontend apache_front
        # Frontend listen port - 80
        bind *:80
        # Set the default backend
        default_backend    apache_backend_servers
        # Enable send X-Forwarded-For header
        option             forwardfor
  
     # Define backend
     backend apache_backend_servers                                                                                                                     
     # Use roundrobin to balance traffic
     balance            roundrobin
     # Define the backend servers
        server             web1 192.168.56.11:80 check
        server             web2 192.168.56.12:80 check
-----------------------------



