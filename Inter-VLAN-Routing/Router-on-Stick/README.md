VLAN 10 = HR            ( f0/1- 4 )             192.168.10.0/24
VLAN 20 = Admin         ( f0/5 -9 )             192.168.20.0/24
VLAN 30 = IT            ( f0/10 -15)            192.168.30.0/24
Management = VLAN 99
192.168.99.0/24

#PC connected with SW1
PC1 = 192.168.10.10
PC2 = 192.168.20.10
PC3 = 192.168.30.10

#PC connected with SW2
PC4 = 192.168.10.11
PC5 = 192.168.20.11
PC6 = 192.168.30.11

##SWITCH_2:

enable secret Cisco123
password I_014!"H
SVI 192.168.99.2/24

##SWITCH_1:

enable secret Cisco123
password I_014!"H
SVI 192.168.99.3/24
