Main idea, we modify NVME pci id's to look like a white-list allowed device (i.e. modem/wifi/etc)

First we should find white-listed VID/PID/SSVID/SSPID, for thinkpad x13s i just read a bios firmware and found this part
of data, well it looks like  id's and repeated like a list of them 
```
0000000001000000 5B10 C1E0 CB17 0C01
0000000001000000 5B10 C2E0 CB17 0C01
0000000001000000 5B10 C3E0 CB17 0C01
```
mirror them »»»»
```
0000000001000000 105B E0C1 17CB 010C
0000000001000000 105B E0C2 17CB 010C
0000000001000000 105B E0C3 17CB 010C
```
yes! i googled this and verify what that's it.
`105B/E0C1/17CB/010C`
`VID/PID/SUBVID/SUBPID`

Then we should find capatible nvme drive for flashing, thinkpad x13s have two m.2 2242 ports: 
first one without white-list have 4 pci line is used for main nvme, 
second one with white-list is key_B port for 5g modem with 2 pcie line, 

I found NETAC N930ES 2242 drive with both of keys(M+B) and compatible for flashing sm2263XT controller + YMTC memory cips.
using a SMI MPTool i changed drive id's to correct one and succefully pass white-list and boot the system,
Windows and Linux OS recognise drive correctly, also it shows in bios screen in same way like main nvme.

here is a details, SSD-X13s-256G - is out patient:
```# lspci -nn
0002:00:00.0 PCI bridge [0604]: Qualcomm Technologies, Inc Device [17cb:010e]
0002:01:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller 980 [144d:a809]
0004:00:00.0 PCI bridge [0604]: Qualcomm Technologies, Inc Device [17cb:010e]
0004:01:00.0 Non-Volatile memory controller [0108]: Foxconn International, Inc. Device [105b:e0c1] (rev 03)
0006:00:00.0 PCI bridge [0604]: Qualcomm Technologies, Inc Device [17cb:010e]
0006:01:00.0 Network controller [0280]: Qualcomm Technologies, Inc QCNFA765 Wireless Network Adapter [17cb:1103] (rev 01)

# nvme list
Node                  Generic               SN                   Model                                    Namespace Usage                      Format           FW Rev  
--------------------- --------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
/dev/nvme1n1          /dev/ng1n1            O83RQ1               SSD-X13s-256G                            1         256.06  GB / 256.06  GB    512   B +  0 B   U0806V0 
/dev/nvme0n1          /dev/ng0n1            S65DNE1RA96913       SAMSUNG MZALQ512HBLU-00BL2               1         178.67  GB / 512.11  GB    512   B +  0 B   5L2QFXM7

```
Cheers cmrds!
