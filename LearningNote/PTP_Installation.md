# 1 .Install PTP 
The required packages are missing on the freshly reinstalled system. You can rebuild the environment by following the steps below.
```
sudo apt update 
sudo apt install linuxptp -y  # Install linuxptp

sudo apt install vim -y  # Install vim
sudo vim /etc/ptp4l.conf
```
## 2 .ptp4l.conf
**Copy & Paste** ptp config blew to `ptp4l.conf`

```
[global]
domainNumber           24
slaveOnly              1
time_stamping          hardware
tx_timestamp_timeout   20
logging_level          6
summary_interval       0

[ens1f1np1]
network_transport      L2
hybrid_e2e             0
```
`:wq` save and quit

> [!tip]
> - **domainNumber**: 24
> - **network_transport**: L2
> - **time_stamping**: hardware
> - **tx_timestamp_timeout** 1
>      - When use E810 NIC the tx_timestamp_timeout need to set to 50 or 100 to successful run ptp4l.
> - **PTP grandmaster** is reachable via interafce **ens1f1np1**(need to change to your interface linked to TM500)

## 3 .Stop NTP

If your *rms > 100 ms* or *phc offset > 100*, it is recommended to perform the following action – **Disable Network Time Protocol (NTP)**, because having both **PTP** and **NTP** enabled at the same time can result in two conflicting clocks, which may cause confusion or malfunction in ptp4l or phc2sys.

```
#Disable Network Time Protocol (NTP)
#to check there is NTP enabled or not
sudo timedatectl | grep NTP
// NTP service: active
#to disable
sudo timedatectl set-ntp false
```
Check after setting false
```
sudo timedatectl | grep NTP   
      NTP service: inactive
```

## 4 .Start ptp4l & phc2sys
```
sudo ptp4l -i ens1f1np1 -m -H -2 -s -f /etc/ptp4l.conf     # start ptp4l
```
<img width="751" height="224" alt="image" src="https://github.com/user-attachments/assets/0b6e0018-0369-4cc9-94e6-308efa75c3ea" />

start `ptp4l` successfully

- Run phc2sys
```
sudo phc2sys -w -m -s ens1f1np1 -R 8 -f /etc/ptp4l.conf
```

-------------------------------------------------------------------

### 5 .Turn ptp4l/phc2sys into systemd service

1. Create or modify a systemd service

- **ptp4l**
```
sudo nano /etc/systemd/system/ptp4l.service
```
```
[Unit]
Description=Precision Time Protocol (PTP) service
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
EnvironmentFile=-/etc/default/ptp4l
ExecStart=/usr/sbin/ptp4l $OPTIONS

[Install]
WantedBy=multi-user.target
```

Configure parameters
```
sudo nano /etc/default/ptp4l
```
```
OPTIONS="-f /etc/ptp4l.conf -i ens1f1np1 -m -H"
```

- **phc2sys**
  
```
sudo nano /etc/systemd/system/phc2sys.service
```
```
[Unit]
Description=Synchronize system clock or PTP hardware clock (PHC)
After=ptp4l.service

[Service]
Type=simple
EnvironmentFile=-/etc/default/phc2sys
ExecStart=/usr/sbin/phc2sys $OPTIONS

[Install]
WantedBy=multi-user.target
```

Configure parameters
```
sudo nano /etc/default/phc2sys
```
```
OPTIONS="-a -r -r -n 24"
```

2. Reload systemd config
```
sudo systemctl daemon-reload
```

3. Start the configured services
```
sudo systemctl start ptp4l.service
sudo systemctl start phc2sys.service
```

4.Check the services state
```
sudo systemctl status ptp4l.service
sudo systemctl status phc2sys.service
```

5. Enable services to automatically restart them after reboot
```
sudo systemctl enable ptp4l.service
sudo systemctl enable phc2sys.service
```
