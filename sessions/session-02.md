# Session 2: Getting Proxmox ready

Date: [21/10/2026]

## What I wanted to do
Get my new Proxmox server updated and set up so it could run all the time.

## What I did
- Proxmox comes set up to get updates from a paid "enterprise" source. I switched it
  to the free "No-Subscription" source and updated the system.
- Changed a setting so the laptop doesn't go to sleep when I close the lid, so I can
  leave it running in the corner.
- Started planning how to build a separate lab network inside Proxmox.

## What went wrong and how I fixed it
My first update failed. I didn't know why at first, but reading the output line by line
showed two sources saying `401 Unauthorized` - the paid ones, which were still switched on:

```
Err:5 https://enterprise.proxmox.com/debian/ceph-squid trixie InRelease
  401  Unauthorized
Err:6 https://enterprise.proxmox.com/debian/pve trixie InRelease
  401  Unauthorized
```

I switched them off properly and the update worked.

I also couldn't log in at first because I typed "admin" as the username.
On Linux the admin account is called "root".

## What I learned
- Error messages are worth reading carefully - the answer was right there in the output
- Proxmox is built on Debian Linux
- Basic Linux commands: `grep` to search a file, `systemctl` to restart a service
- In Linux config files, a line starting with `#` is switched off

## Next
Set up a virtual router (OPNsense) and a separate lab network.
