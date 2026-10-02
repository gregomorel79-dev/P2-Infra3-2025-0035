# oct/02/2026 03:32:15 by RouterOS 6.49.17
# software id =
#
#
#
/interface ethernet
set [ find default-name=ether1 ] disable-running-check=no
set [ find default-name=ether2 ] disable-running-check=no
set [ find default-name=ether3 ] disable-running-check=no
set [ find default-name=ether4 ] disable-running-check=no
set [ find default-name=ether5 ] disable-running-check=no
set [ find default-name=ether6 ] disable-running-check=no
set [ find default-name=ether7 ] disable-running-check=no
set [ find default-name=ether8 ] disable-running-check=no
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip ipsec profile
add dh-group=modp2048 enc-algorithm=des name=fg-p1
/ip ipsec peer
add address=200.35.0.1/32 name=FG1 profile=fg-p1
/ip ipsec proposal
add enc-algorithms=des lifetime=12h name=fg-p2 pfs-group=modp2048
/ip address
add address=200.35.0.3/24 interface=ether1 network=200.35.0.0
add address=10.0.35.1/28 interface=ether2 network=10.0.35.0
add address=10.0.36.1/24 interface=ether3 network=10.0.36.0
/ip dhcp-client
add disabled=no interface=ether1
/ip firewall nat
add action=accept chain=srcnat dst-address=10.0.35.128/25 src-address=10.0.36.0/24
add action=accept chain=srcnat comment="No NAT hacia VPN" dst-address=10.0.35.128/25 src-address=10.0.35.0/28
add action=masquerade chain=srcnat comment="NAT salida ISP" out-interface=ether1
/ip ipsec identity
add peer=FG1 secret=Lab2025-0035
/ip ipsec policy
add disabled=yes dst-address=10.0.35.128/25 peer=FG1 proposal=fg-p2 src-address=10.0.35.0/28 tunnel=yes
add dst-address=10.0.35.128/25 peer=FG1 proposal=fg-p2 src-address=10.0.36.0/24 tunnel=yes
/ip route
add comment="Default ISP" distance=1 gateway=200.35.0.1
add comment="Hacia usuarios via VPN" distance=1 dst-address=10.0.35.128/25 gateway=200.35.0.1
/system identity
