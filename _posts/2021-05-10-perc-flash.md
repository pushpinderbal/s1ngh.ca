---
layout: post
title:  "Flash Perc H710P Mini RAID Controller to IT Mode"
author: "Pushpinder"
date:   2021-05-10 23:08:10 +0000
---

The Perc H710P controller in this case installed in my R720 didn't have the capability to mount single disks. By flashing the controller in IT mode, the controller is considered as a general LSI controller for hard drives.
<p class="callout info">SCSI drives can fit into SAS ports, but the reverse is not true.</p>

To flash the controller, it will require a FreeDOS and a Linux ISO pre-built with the firmware. The ISOs can be downloaded from this link: **[https://fohdeesha.com/docs/perc/](https://fohdeesha.com/docs/perc/)**

1. To begin flashing the controller, remove the battery from the controller as the IT firmware wouldn't even recognize the battery.
2. Flash the downloaded ISOs to a USB. (I tried Ventoy to use both ISOs from the same USB, but it didn't seem to support these OS builds).
3. Boot the FreeDOS image and get the card details to perform the appropriate steps. **Take note of the SAS address.**
    - `info`
4. Run the pre built commands to flash the firmware after identifying the correct card. For example, to flash the H710P D1 Mini: 
    - `PD1CROSS`
    - `reboot`
5. Boot into the Linux live environment and start a root shell.
6. Run the Flashing script. 
    - `D1-H710`
7. If the above finished successfully, reboot to the Linux image and program the SAS address back.
8. To program the SAS address back, start a root shell and run the command with SAS ID matching as noted in Step 3. 
    - setsas *SAS\_ID*
9. If you need to boot from the drives connected to the controller, flash a boot image. 
    - Flash BIOS image - `flashboot /root/Bootloaders/mptsas2.rom`
    - Flash UEFI image - `flashboot /root/Bootloaders/x64sas2.rom`

<p class="callout info">With my testing, when the controller is flashed to IT mode, it randomly assigns an order to the drive bays on the server. You will need to figure out which bay is showing up first in the controller boot order, and then make sure that the boot drive is plugged into that. In the boot menu, all the drives in the controller were presented as just one entry, which is why we need to make sure that we have the boot disk in the right bay which is being shown in the boot menu.</p>

All the above documentation is an extract from fohdeesha's page. For more information, please go to the link for the docs on his website using the link above.

Big thanks to Fohdeesha for making the above possible and he deserves all the credit.