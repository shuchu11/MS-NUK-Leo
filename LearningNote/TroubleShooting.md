## How to check your NIC
After checking the kernel's logs (`sudo demsg`) , I find there are some issue on NIC

- LOGs -  `sudo demsg`
<img width="1263" height="231" alt="image" src="https://github.com/user-attachments/assets/dae5f3c4-3b83-4533-8cc4-7a8026efd685" />

Check NIC
```
ip link show ens1f1
```

It woutld show info blew (vf0 & vf1 are NICs)

```
5: ens1f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 50:7c:6f:3b:de:8b brd ff:ff:ff:ff:ff:ff
    vf 0     link/ether 00:11:22:33:44:88 brd ff:ff:ff:ff:ff:ff, vlan 564, qos 1, spoof checking on, link-state auto, trust off
    vf 1     link/ether 00:11:22:33:44:89 brd ff:ff:ff:ff:ff:ff, vlan 564, qos 1, spoof checking on, link-state auto, trust off
    altname enp112s0f1
```

