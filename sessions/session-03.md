# Session 3: Building a router and a lab network

Date: [22/10/2026]

## What I wanted to do
Create a separate lab network inside Proxmox, with its own router, so I could
experiment without affecting the rest of my home network.

## What I did
- Created a second virtual switch in Proxmox (vmbr1) that isn't connected to
  anything physical, just for the lab
- Installed OPNsense, a free router/firewall, as a virtual machine. It has two
  network cards: one on my home network (WAN) and one on the lab network (LAN)
- Gave the lab its own address range, 10.10.10.0/24, with OPNsense handing out
  addresses automatically (DHCP)
- Installed Linux Mint as a second virtual machine on the lab network, to act as
  a client inside the lab
- Checked both downloads with SHA-256 hashes before installing them, to make sure
  the files were genuine and not corrupted

## What went wrong and how I fixed it

**The router's network cards were the wrong way round.**
After installing, OPNsense had put its LAN side on my home network and given itself
192.168.1.1, with its own DHCP server switched on. That meant it could have started
giving out addresses to phones and laptops in my house, clashing with my real router.
I swapped the two cards around from the OPNsense console, and gave the lab its own
address range (10.10.10.x), because the lab and home networks can't use the same range.

**Installers were running out of memory.**
OPNsense warned it needed more memory to install, and later the Linux Mint installer
got stuck. In Proxmox I could see the Mint VM was using about 93% of its memory.
I gave both VMs more memory just for installing, then turned it back down afterwards.

**The clock in Mint showed the wrong time.**
It was an hour behind because the timezone was set to UTC instead of Dublin.
The underlying clock was correct (synced over NTP), so only the displayed time was
wrong. Choosing Dublin during the install fixed it. Even a timezone mistake matters:
it makes log timestamps confusing when comparing devices, and scheduled tasks run at
the wrong time. If the actual clock is wrong, things like HTTPS certificates and
DNSSEC can fail completely, because they depend on accurate time.

## Testing it
From the Mint VM I pinged the router, then an internet address (8.8.8.8), then a
website name (google.com). All three worked, so the router, the internet connection
and DNS were all working.

Then I ran `tracepath` to see the route my traffic takes:

```
1:  10.10.10.1      <- my OPNsense router
2:  192.168.1.254   <- my home router
3:  (ISP network)
```

Traffic goes through two routers before it leaves my house (this is called double NAT).

## What I learned
- How a router sits between two networks and passes traffic between them
- Why two networks connected by a router need different address ranges
- How to read `ip a`, `ping` and `tracepath` output
- To check resource usage when something is slow, instead of guessing
- My lab's main limit is RAM (8 GB). There's a free slot, so I can add another 8 GB

## Next
Finish setting up the Mint VM, then connect real hardware to the lab
(my managed switch and an old laptop) once my USB network adapters arrive.
