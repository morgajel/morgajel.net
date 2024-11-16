---
author: Jesse Morgan
categories:
- Uncategorized
date: "2019-03-20T19:27:57Z"
guid: http://morgajel.net/?p=1761
id: 1761
title: proxmox PCI passthrough with windows, Geforce 1070, Ryzen, and B450 Tomahawk
url: /2019/03/20/1761
---

I set up my first Proxmox implementation on my rebuilt gaming PC. The goal was to run proxmox on bare metal, then run a windows VM with hardware passthrough so I could play Elite Dangerous in windows with only a 1-3% performance loss. This would also give me a platform to work on automation tools and containerization.

So how did I go about doing it? Well, I started by reading this article: https://techblog.jeppson.org/2018/03/windows-vm-gtx-1070-gpu-passthrough-proxmox-5/

That did most of the heavy lifting, but it was specific to intel processors. Here’s what my final changes looked like:

# BIOS

I needed to enable 3 main things:

- WHQL for windows 10
- UEFI Bios
- enable virtualization under the Overclocking-&gt; CPU Features panel

GRUB

/etc/default/grub needs to have the following DEFAULT line:

GRUB\_CMDLINE\_LINUX\_DEFAULT=”quiet amd\_iommu=on iommu=pt video=efifb:off”

Modprobe blacklist

/etc/modprobe.d/blacklist.conf needs the following entry:

blacklist radeon  
blacklist nouveau  
blacklist nvidia  
blacklist amdgpu

# QEMU Host config

/etc/pve/nodes/moxie/qemu-server/101.conf

```
agent: 1 
bios: ovmf 
bootdisk: scsi0 
cores: 8 
cpu: host,hidden=1
hostpci0: 1c:00.0,x-vga=on,pcie=1 
hostpci1: 1c:00.1 
hostpci2: 1d:00.3 
hostpci3: 1e:00.3,pcie=1 
ide2: local:iso/virtio-win-0.1.141.iso,media=cdrom,size=309208K 
machine: q35 
memory: 12000 
name: gamey 
net0: e1000=DE:F7:85:97:FF:22,bridge=vmbr0 
numa: 1 
onboot: 1 
ostype: win10 
scsi0: local-lvm:vm-101-disk-0,size=100G 
scsihw: virtio-scsi-pci 
smbios1: uuid=d0e62ae5-0939-4544-aa2e-7e92f872cc39 
sockets: 1 
usb0: host=1-2 
usb1: host=0c45:7605 
usb2: host=046d:c332 
virtio2: /dev/disk/by-id/ata-CT500MX500SSD1_1817E1395213-part1,size=476937M 
vmgenid: fa74f2e1-46d1-444b-963a-1f0417d18fd0
```

# /etc/modprobe.d/vfio.conf

```
options vfio-pci ids=10de:1b81,0de:10f0
```

I apologize that this is super rough and poorly formatted, but I figured that was better than nothing.