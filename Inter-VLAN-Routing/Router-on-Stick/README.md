VLAN 10 = HR            ( f0/1- 4 )             192.168.10.0/24
<br>
VLAN 20 = Admin         ( f0/5 -9 )             192.168.20.0/24
<br>
VLAN 30 = IT            ( f0/10 -15)            192.168.30.0/24
<br>
Management = VLAN 99
<br>
192.168.99.0/24
<br><br>

#PC connected with SW1 <br>
PC1 = 192.168.10.10 <br>
PC2 = 192.168.20.10 <br>
PC3 = 192.168.30.10 <br><br>

#PC connected with SW2 <br>
PC4 = 192.168.10.11<br>
PC5 = 192.168.20.11 <br>
PC6 = 192.168.30.11 <br><br>

##SWITCH_2:<br>

enable secret Cisco123<br>
password I_014!"H <br>
SVI 192.168.99.2/24<br>

##SWITCH_1:

enable secret Cisco123
password I_014!"H
SVI 192.168.99.3/24
