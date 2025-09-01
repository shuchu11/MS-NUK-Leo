```
sudo nano /etc/resolv.conf
```
Please change the `nameserver` to the correct DNS\
(EX: `DNS` = 192.168.8.72)
```
nameserver 192.168.8.72
```

Check the DNS status
```
resolvectl status

>> It would show as blew

Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (eno1)
Current Scopes: none
     Protocols: -DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported

Link 3 (eno2)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 140.118.31.99
       DNS Servers: 8.8.8.8 140.118.31.99 192.168.8.72   -----------------------------> "192.168.8.72" has been successfully written into the file

```

You can also use the command blew to check
```
ubuntu@ubuntu-SYS-111E-WR:~/O-DU-High$ nslookup git.bmw.lab
Server:         192.168.8.72
Address:        192.168.8.72#53

Name:   git.bmw.lab
Address: 10.1.0.42
```
