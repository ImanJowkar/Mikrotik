# MTCNA

* What is mikrotik: `MikroTik is a company based in Latvia.`

* what is differece between router-os vs router board
* `package section in version-6 vs version-7`

* product Naming


## start - Basic configuration

* reset configuration, and change the identity

```
system reset-configuration no-defaults=yes skip-backup=yes
system/identity/set name=R1

```

* `always use Safe Mode`: When you enable this feature, it will create a snapshot of the configurations. If your session times out, the configuration will revert to the backup.
* `Disable unused services`
```
ip service/print
ip service/disable ftp
ip service/disable telnet

```
* change default ports
* work with ftp, allow ftp only from white-list ip address
```
ip service set ftp address=192.168.229.0/2

```

* add disk and write file-system into it and connect to this with ftp



## Assgin Ip address on interfaces
* always change the name of interfaces 
```
# dhcp-client
ip dhcp-client/set interface=ether1 add-default-route=yes use-peer-ntp=yes use-peer-dns=yes


# static ip addressing
ip address/add interface=ether3 address=10.10.10.1/24






```

* disable neighbor discovery

```
# disallow on all interfaces
/ip neighbor discovery-settings set discover-interface-list=none


# allow on specific interface list, 

/interface list add name=trust-list
/interface list member add interface=ether2 list=trust-lis
/ip neighbor discovery-settings set discover-interface-list=trust-list


```


* you can use ip scan to find ip addresses used in your network
```

tool/ip-scan interface=ether1
```




# syslog-grafana

```
# synchronize the date and time in your device
/system ntp client set enabled=yes
/system ntp client servers add address=ntp.day.ir
/system clockset time-zone-name=Asia/Tehran


/system logging action set 3 remote=<syslog-server-ip> src-address=<src-ip-address>
/system logging
set 0 action=remote
set 1 action=remote
set 2 action=remote
set 3 action=remote






# enable logging on firewall 

/ip firewall filter add action=log chain=input log=yes log-prefix="wowwwwwwww some one ping router" protocol=icmp
/ip firewall filter add action=add-src-to-address-list address-list=ping-router address-list-timeout=none-static chain=input protocol=icmp










```