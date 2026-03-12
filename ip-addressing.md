## Vlan IP Addressing Plan
> All addresses to be statically allocated aside from the IoT devices in vlan4. 

**vlan"0" - management**
- vlan"0" int: 10.0.x.1
- iDRAC8: 10.0.x.80
- HP managed switch: 10.0.x.40
- UniFi WAP controller VM: 10.0.x.20
- UniFI WAP device: 10.0.x.30

**vlan1 - Server**
- vlan1 int: 10.0.1.1
- 4x1Gb NIC (EtherChannel/LACP): 10.0.1.10
  > Port Channel will be tagged with both vlan1 and vlan"0" to allow management access.
- UniFi WAP controller VM: 10.0.1.20

**vlan2 - Semi Trusted**
- vlan2 int: 10.0.2.1
- Smart TV *{~wired}*: 10.0.2.10
- Nintendo DS: 10.0.2.20
- PS3 *{~wired}*: 10.0.2.30
- Nintendo 3DS: 10.0.2.40
- PS5 *{~wired}*: 10.0.2.50
- Nintendo Switch: 10.0.2.60
- Old Tower1: 10.0.2.150
- Old Laptop1: 10.0.2.200

**vlan3 - Trusted**
- vlan3 int: 10.0.3.1
- Chromebook: 10.0.3.10
- Laptop: 10.0.3.30
- Phone1: 10.0.3.50
- Phone2: 10.0.3.70
- Tablet: 10.0.3.90

**vlan4 - IoT/No Trust**
- vlan4 int: 10.0.4.1
- Google Home Mini: 10.0.4.10
- Google Nest Hub: 10.0.4.20
- Ikea Gateway *{~wired}*: 10.0.4.30
- IoT bulbs/switches/sensors: DHCP pool-10.0.4.100-199
