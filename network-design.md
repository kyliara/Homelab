## Network Designs
>This documents the home network redesign for a single-room homelab environment. The primary goals are: VLAN segmentation, separation of trusted/semi-trusted/untrusted devices, a managed server environment running Proxmox VE, and remote access via Tailscale. Hardware includes a Dell PowerEdge R730, HP V1910-48G managed switch, and UniFi U6+ wireless AP.
>Future plans include adding DHCP server VM to manage addressing. 

## Current Design
**Google Wifi Router**
- Ikea Gateway (directly connected)

**Google Wifi Point**
- *All* wireless devices
> No network isolation. Excess of IoT devices and mix of trusted/untrusted devices on same flat network. Bandwidth/coverage issues due to excess of devices. 

## New Design - Now with vlan! (3.2026)
*Devices marked as {~wired} are those with Ethernet connections, rather than wireless.*

**vlan"0" - management**
- iDRAC8
- HP managed switch
> Fully isolated vlan for switch and server management interfaces. Only devices with explicit interfaces on this vlan can reach it. ACLs will enforce no cross-vlan access to or from this segment.

**vlan1 - Server**
- 4x1Gb NIC (EtherChannel/LACP)
> Four physical NICs bonded via LACP to improve aggregate bandwidth and provide redundancy in case of individual physical port/cable failure. Standard production pattern for server uplinks.

**vlan2 - Semi Trusted**
- PS3 *{~wired}*
- PS5 *{~wired}*
- Nintendo Switch
- Nintendo DS & 3DS
- Smart TV *{~wired}*
- Old Tower1 
- Old Laptop1

**vlan3 - Trusted**
- Phones
- Chromebook
- Laptop
- Tablet

**vlan4 - IoT/No Trust**
- Google Home Mini
- Google Nest Hub
- Ikea Gateway *{~wired}*
- IoT bulbs
- IoT switches
- IoT sensors
> Google Home Mini, Nest Hub, and Ikea Gateway placed in vlan4 to access the IoT devices they're meant to control. IoT devices use mDNS over a link-local multicast for discovery between devices and hubs. If these hubs are placed in a separate segment, they won't be able to communicate with their current or future devices in a different segment, as the link-local does not cross vlan borders. 

## New Topology (3.2026)
> Original Google Wifi does not support vlan tagging (802.1q). Managed switch and WAP will handle all tagging and inter-vlan routing. 

**Google Wifi Router** {uplink to landlord's router}
- local gateway
> Using Tailscale to traverse double NAT caused by two routers in series, facilitating access into network by trusted devices. 

**Google Wifi Point** {physical link to HP Switch}
- in a wireless mesh with the Google Wifi Router
> Originally needed as a range extender due to building design obstructing wireless signal, now used for Ethernet access. 

**HP managed Switch**
- `G1/0/1` iDRAC8
- `G1/0/3-G1/0/6` Dell R730 Server {LACP bond}
- `G1/0/17` PS3
- `G1/0/18` PS5
- `G1/0/31` Smart TV
- `G1/0/33` Ikea Gateway
- `G1/0/45` Managed WAP {Trunked port!} 
- `G1/0/47` Uplink to Google Wifi Point (untagged, Google Wifi does not support 802.1q trunking)

>[Physical segmentation to mirror vlan segmentation: port 1 for vlan "0," ports 3-16 for vlan1, ports 17-24 for vlan 2, ports 25-32 for vlan 3, ports 33-44 for vlan 4, ports 45-48 for uplink]

**UniFi managed WAP**
- Separate SSID for each vlan
- All devices connect to SSID corresponding to their designated vlan group

**UniFi Controller VM (Proxmox)**
- `vNIC0`: vlan"0" (management/controller -> WAP communication only)
- `vNIC1`: vlan1 (server network, primary)

>[The controller runs on vlan1 (server) but needs to reach the managed WAP which sits on vlan"0" (management). Rather than routing between vlans, the controller VM gets a second virtual NIC to access vlan"0" directly. Management traffic stays on the management vlan without punching holes in the segmentation.]

*This is a personal homelab environment used for hands-on learning. Configuration choices prioritize understanding over operational simplicity.*

*Updated 3/9/2026*
