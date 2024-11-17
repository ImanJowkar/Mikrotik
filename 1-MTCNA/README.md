# MTCNA

* What is mikrotik: `MikroTik is a company based in Latvia.`

* what is differece between router-os vs router board
* `package section in version-6 vs version-7`



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

## give Internet access to your clients
![img](img/1.png)

```



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
## manage user and groups in mikrotik

```


```




# syslog-grafana

```
# synchronize the date and time in your device
/system ntp client set enabled=yes
/system ntp client servers add address=ntp.day.ir

# v6
/system clockset time-zone-name=Asia/Tehran

# v7
system/clock/set time-zone-name=Asia/Tehran

/system logging action set 3 remote=<syslog-server-ip> src-address=<src-ip-address>
/system logging
set 0 action=remote
set 1 action=remote
set 2 action=remote
set 3 action=remote






# enable logging on firewall 

/ip firewall filter add action=log chain=input log=yes log-prefix="wowwwwwwww some one ping router" protocol=icmp
/ip firewall filter add action=add-src-to-address-list address-list=ping-router address-list-timeout=none-static chain=input protocol=icmp







# LOGQL


{hostname="10.10.1.1"}

{hostname=~".+", hostname!="192.168.229.170"}   # all logs but not for "192.168.229.170"

{hostname=~".+", hostname!="192.168.229.170"} |= "down"
{hostname=~"10.10.1.100|10.11.1.2"} |= "down"

{hostname=~".+"} |~ "[Dd]own"   # search for down or Down
{hostname=~".+"} |~ "down|Down"  # same as above




# SSH dashboard 

{hostname=~".+"} |~ "ssh|SSH"
{job="syslog"} |~ "ssh|SSH" |~ "Failed"

{job="syslog"} |~ "ssh|SSH" |~ "Failed" |~ "(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)"




{job="syslog"} |= "Failed" 



# annotation
{hostname=~".+"} |~ "ssh|SSH"  |~ "Failed"

```


## upgrade-downgrade(2-40)



## desgin skin (only works on web interface)
login to user mikrotik on web ui with admin priveleges, and create new skin(in the Desgin Skin section)


## Backup and Restore
we have two types of backups
* full backup (good for router itself)   .backup
* specific (export)           .rsc

1) full backup
![img](img/2.png)
```
# backup
go to the files and click on backup(you can set password on the backup file too.)


# restore go to the files and choose a file and then click on restore and reboot the system
```

1) specific backup
```
# Backup
export      # all static configuration

ip address/export
ip firewall/export file=firewall.rsc
mpls/export file=mpls.rsc



# Restore
import firewall.rsc

```

## Netinstall

## product Naming
