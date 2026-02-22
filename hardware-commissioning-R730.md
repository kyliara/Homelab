# Hardware Commissioning Log: Dell PowerEdge R730

## 1. Initial Inspection & Teardown
**Date:** Feb 21, 2026

* **Exterior:** Cosmetic damage observed to center of lid along length; structural integrity intact. 
* **Rear I/O & Power:** Removed, inspected, and reseated PSU1 and PSU2.
* **Front Chassis:** Removed and inspected all drive caddies. Hardware packet (screws) taped to caddy in drive slot 0.
* **Interior Chassis:** Grounded to chassis, removed lid and fan shroud. 
  * Performed visual inspection of mainboard, PCIe risers, fans, backplane, RAM, heat spreaders, CMOS battery, PERC battery, and controller. No faults observed. 
  * Performed olfactory inspection of interior and visual inspection of all capacitors; no blown components detected. 
  * Recorded sticker serial numbers across components.
* **Front Panel LCD:** Observed error for PSU2 power loss. Confirmed PSU2 intentionally unpowered; manually cleared error code via front panel interface.

## 2. Hardware Inventory & Specifications
* **CPU:** 2x Intel Xeon E5-2687 v3 @ 3.10 GHz
* **Memory:** 64GB (4x 16GB) @ 2133 MHz | Populated Slots: A1, A2, B1, B2 | PN: 18ASF2G72PDZ-2G3B1
* **Storage:** * Controller: PERC H730P Mini (Firmware: 25.5.9.0001 [Verified current 2.21.2026]
  * Drive Bays: 8x 2.5" SFF (Empty) | PN: HDEBF05DAA51 | MN: AL14SEB030N
* **Network Interfaces:** Quad port 1GbE (Firmware: 20.2.17) + Dedicated iDRAC port
* **Power Supply:** * PSU 1: 750W Platinum 80 (Firmware: 00.23.32) - Present / OK
  * PSU 2: 750W Platinum 80 - Present / Unprovisioned (No power cable physically connected for lab environment)

## 3. iDRAC Initialization & Networking
**Connection:** Direct Ethernet (Laptop-to-Server) via USB-C adapter and legacy Cat5 patch cable (bottleneck noted).

* **Laptop IP:** 192.168.0.122
* **iDRAC IP:** 192.168.0.120 (Confirmed via front LED panel)
* **Subnet Mask:** 255.255.255.0

**Remote Out-of-Band (OOB) Management:**
* **Routing Architecture:** Debian-based Subnet Router
* **Tunneling Protocol:** Tailscale
* **Access Control:** Bridged isolated physical management interface to external administrative terminal via secure, encrypted tunnel.

**Firmware Baselines (Pre-Update):**
* iDRAC / Lifecycle Controller: 2.43.43.43
* BIOS: 2.4.3
* uEFI Diagnostics: 4239A36

## 4. Active Fault Diagnostics: Cooling System
**Issue Discovery:** Uneven resonance from cooling fans detected audibly during idle runtime.

**iDRAC Telemetry:**
* Fan 1-4, 6: 3600 RPM
* Fan 5: 3720 RPM (Persistent)
* Fan 3: 3720 RPM (Intermittent spike)
* Temperatures: Inlet 21°C | Exhaust 32°C | CPU1 55°C | CPU2 56°C

**Diagnostic Notes:** Audible wobble; fans cycling into and out of resonance indicating imbalance or bearing wear. 

**Remediation Plan:**
1. Document  and logcurrent baseline in iDRAC.
2. Power down and isolate server.
3. Visually inspect Fan 3 and Fan 5 for dust accumulation, loose mounting, or physical obstruction.
4. Reseat identified fan modules to ensure proper contact.
5. Boot and verify resonance. 
6. Procure replacement modules if fault persists post-reseat.
