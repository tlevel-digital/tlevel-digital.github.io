# Inspecting System Hardware

In both Windows and Linux, there are multiple ways of exploring devices. Finding their details, drivers, and features. Under Windows there are a few tools that we can use:

Accessible by right-clicking on the start icon:

* Device Manager
* Disk Manager

Accessible via the run dialog: 

* DXDIAG
* winver

Under Linux, most of these tools are presented as CLI alternatives:

Explore your systems with the following tools:

```bash
lspci -v

lsusb

lscpu

free -h

dmesg 

journalctl -xe

systemd-analyze blame
```
