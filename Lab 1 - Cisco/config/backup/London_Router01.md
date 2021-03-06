#  London_Router01's Hardware/Software Information
```bash
Router London_Router01 is started
  Running on server netlab with port 3080
  Local ID is 8 and server ID is 640edff0-9307-42af-a341-9ba4611ddebd
  Dynamips ID is 3
  Hardware is Dynamips emulated Cisco c3725  with 256MB RAM and 256KB NVRAM
  Console is on port 5067 and type is telnet, AUX console is on port None
  IOS image is "c3725-adventerprisek9-mz.124-15.T14.image"
  with no idlepc value
  PCMCIA disks: disk0 is 0MB and disk1 is 0MB
    slot 0 hardware is GT96100-FE with 2 ports
      FastEthernet0/0 connected to Core_S2 on port Gi0/0
      FastEthernet0/1 connected to Switch1 on port Ethernet0
    slot 1 hardware is NM-1FE-TX with 1 port
      FastEthernet1/0 connected to CiscoASA on port Gi0/1
    slot 2 hardware is NM-4T with 4 ports
      Serial2/0 connected to R1 on port Serial2/0
      Serial2/1 is empty
      Serial2/2 connected to R2 on port Serial2/2
      Serial2/3 is empty
    WIC-1T installed in WIC slot 0 with 1 port
      Serial0/0 is empty
    WIC-2T installed in WIC slot 1 with 2 ports
      Serial0/1 is empty
      Serial0/2 is empty
```
### 1.1. London_Router01's configuration
```bash
version 12.4
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname London_Router01
!
boot-start-marker
boot-end-marker
!
logging buffered 4096 critical
logging console alerts
enable password 7 0208014F07070D
!
aaa new-model
!
!
aaa authentication login default local
aaa authorization exec default local none
!
!
aaa session-id common
memory-size iomem 5
clock timezone GMT 1
no ip icmp rate-limit unreachable
ip cef
!
!
no ip dhcp use vrf connected
ip dhcp excluded-address 10.1.56.1
ip dhcp excluded-address 10.1.56.129
ip dhcp excluded-address 10.1.3.1
ip dhcp excluded-address 192.168.200.1
ip dhcp excluded-address 10.1.21.1 10.1.21.5
ip dhcp excluded-address 10.1.31.1 10.1.31.5
ip dhcp excluded-address 10.1.81.1
!
ip dhcp pool FirstFloor_VLAN20
   network 10.1.21.0 255.255.255.0
   default-router 10.1.21.1
   domain-name netlab.net
   dns-server 8.8.8.8
!
ip dhcp pool SecondFloor_VLAN30
   network 10.1.31.0 255.255.255.0
   default-router 10.1.31.1
   domain-name netlab.net
   dns-server 8.8.8.8
!
ip dhcp pool Printers_VLAN55
   network 10.1.56.0 255.255.255.224
   default-router 10.1.56.1
   domain-name netlab.net
   dns-server 8.8.8.8
!
ip dhcp pool Utilities_VLAN100
   network 10.1.3.0 255.255.255.128
   default-router 10.1.3.1
   domain-name netlab.net
   dns-server 8.8.8.8
!
ip dhcp pool Wireless_VLAN700
   network 10.1.3.128 255.255.255.128
   default-router 10.1.2.129
   domain-name netlab.net
   dns-server 8.8.8.8
!
ip dhcp pool GUEST_VLAN900
   network 192.168.100.0 255.255.255.0
   default-router 192.168.200.1
   domain-name wr
   dns-server 8.8.8.8
!
ip dhcp pool Printers_VLAN80
   network 10.1.81.0 255.255.255.0
   default-router 10.1.81.1
   domain-name wr
   dns-server 8.8.8.8
!
ip dhcp pool LAN
   network 192.168.80.0 255.255.255.0
   default-router 192.168.80.1
   domain-name netlab.net
   dns-server 8.8.8.8
!
!
ip domain name netlab.net
ip name-server 8.8.8.8
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
!
username netlab privilege 15 password 7 121700031E0A0E
archive
 log config
  logging enable
  hidekeys
!
!
!
!
ip tcp synwait-time 5
ip scp server enable
!
!
!
!
interface Loopback0
 ip address 1.1.1.3 255.255.255.255
!
interface FastEthernet0/0
 description UP Link to Core_S2 on Gi0/0
 ip address 10.1.12.254 255.255.255.0
 duplex auto
 speed auto
!
interface Serial0/0
 no ip address
 shutdown
 clock rate 2000000
!
interface FastEthernet0/1
 description Link to DMZ||Production Servers
 ip address 192.168.80.1 255.255.255.0
 duplex auto
 speed auto
!
interface Serial0/1
 no ip address
 shutdown
 clock rate 2000000
!
interface Serial0/2
 no ip address
 shutdown
 clock rate 2000000
!
interface FastEthernet1/0
 no ip address
 shutdown
 duplex auto
 speed auto
!
interface Serial2/0
 ip address 8.1.2.2 255.255.255.252
 serial restart-delay 0
!
interface Serial2/1
 no ip address
 shutdown
 serial restart-delay 0
!
interface Serial2/2
 ip address 8.1.3.2 255.255.255.252
 serial restart-delay 0
!
interface Serial2/3
 no ip address
 shutdown
 serial restart-delay 0
!
router ospf 1
 log-adjacency-changes
 network 0.0.0.0 255.255.255.255 area 0
!
router bgp 65001
 no synchronization
 bgp log-neighbor-changes
 network 8.1.1.0 mask 255.255.255.252
 network 8.1.2.0 mask 255.255.255.252
 network 8.1.3.0 mask 255.255.255.252
 network 10.1.2.0 mask 255.255.255.0
 network 10.1.3.0 mask 255.255.255.0
 network 10.1.11.0 mask 255.255.255.0
 network 10.1.12.0 mask 255.255.255.0
 network 10.1.20.0 mask 255.255.255.0
 network 10.1.21.0 mask 255.255.255.0
 network 10.1.30.0 mask 255.255.255.0
 network 10.1.31.0 mask 255.255.255.0
 network 10.1.80.0 mask 255.255.255.0
 network 10.1.81.0 mask 255.255.255.0
 network 10.1.250.0 mask 255.255.255.240
 network 10.1.251.0 mask 255.255.255.240
 network 10.20.20.0 mask 255.255.255.0
 neighbor 1.1.1.2 remote-as 65001
 neighbor 1.1.1.2 update-source Loopback0
 neighbor 8.1.2.1 remote-as 65000
 no auto-summary
!
ip forward-protocol nd
ip route 0.0.0.0 0.0.0.0 8.1.2.1
ip route 0.0.0.0 0.0.0.0 10.1.12.1
!
!
no ip http server
no ip http secure-server
!
logging trap debugging
no cdp log mismatch duplex
!
!
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
!
!
line con 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
line vty 0 4
 transport input all
!
ntp server 188.168.3.28
!
end

```
