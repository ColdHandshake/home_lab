# Session 1: Planning, network mapping and Proxmox install

Date: [20/10/2026]

## Hardware
- Main laptop: Windows, Wi-Fi only (no Ethernet port)
- Spare laptop (lab server): Dell Inspiron 15, Intel i7-8550U (4 cores/8 threads),
  8 GB RAM, 250 GB SSD (Proxmox installed), 1 TB HDD (unused for now),
  Realtek gigabit Ethernet (named nic0 in Proxmox), USB-A + USB-C
- Old laptop (~15 yrs): spare, future physical lab client / Wireshark machine
- TP-Link TL-SG108E: 8-port gigabit managed switch (VLANs, port mirroring)
- ISP fibre router: active home internet router
- eir F1000 (Zyxel) + eir F2000 (Huawei): spare, not in use
- Cables: Cat5e UTP, 568B
- To buy: 2x USB gigabit adapters (TP-Link UE300 / UE300C, RTL8153 chipset)

## Home network
- Network: 192.168.1.0/24 (subnet mask 255.255.255.0)
- Gateway + DNS: 192.168.1.254 (ISP router)
- DHCP range: 192.168.1.1 - 192.168.1.200, lease time 1 hour
- Static/free zone: 192.168.1.201 - 192.168.1.253
- Found by logging into the router's admin page and running ipconfig on Windows

## IP address plan
| Address        | Device                          |
|----------------|---------------------------------|
| 192.168.1.254  | ISP router (gateway)            |
| 192.168.1.250  | Proxmox host (spare laptop)     |
| 192.168.1.251  | Reserved: OPNsense WAN (later)  |

## Proxmox install (done)
- Installer USB made with Rufus (DD image mode)
- BIOS: virtualization (VT-x, VT-d) already enabled, SATA already AHCI
- Booted via F12 boot menu -> "UEFI: ... Partition 2" -> Install Proxmox VE (Graphical)
- Disk: /dev/sdb (SSD), Windows removed
- Hostname: pve.lab.home
- Management interface: nic0, IP 192.168.1.250/24, gateway + DNS 192.168.1.254
- Web interface: https://192.168.1.250:8006 (realm: Linux PAM)

## Key concepts learned
- DHCP: router hands out addresses automatically from a set range
- Static IPs go outside the DHCP range to avoid conflicts
- /24 = subnet mask 255.255.255.0
- ping "Destination host unreachable" on local network = no device answered ARP
- Hypervisor (Proxmox) runs virtual machines (VMs) on one physical computer
- ISO = image of an installer disc; Rufus writes it to a bootable USB stick

## Next steps
1. Log into Proxmox web interface from main laptop
2. Switch to the free "No-Subscription" update repository and update
3. Set laptop to ignore lid closing
4. Create OPNsense router VM on an internal virtual network
5. Create a Linux client VM behind OPNsense
6. When adapters arrive: extend lab to the TL-SG108E switch and old laptop
