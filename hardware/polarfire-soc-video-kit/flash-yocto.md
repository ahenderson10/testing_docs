---
title: Flash Yocto Linux
layout: default
parent: PolarFire SoC Video Kit
grand_parent: Hardware
nav_order: 3
---

# Flashing Yocto Linux to the PolarFire SoC Video Kit

The VectorBlox SoC Video Kit demo runs on Yocto Linux **2023.02.1**. This page describes how to update the eMMC with a fresh image using the Hart Software Services (HSS) and USBImager.

{: .note }
On Windows, the Yocto image can also be programmed via USBImager following the [PolarFire SoC documentation](https://github.com/polarfire-soc/polarfire-soc-documentation/blob/v2023.02/reference-designs-fpga-and-development-kits/updating-mpfs-kit.md).

## Prerequisites

- **USBImager** installed on your host PC ([download](https://bztsrc.gitlab.io/usbimager/))
- USB cable connected to **J12** (UART — for HSS access on UART0)
- USB cable connected to **J19** (USB OTG — for eMMC mass storage)
- Serial terminal open at 115200 baud, 8N1, no flow control

## Download the Yocto image

Download the 2023.02.1 Yocto image for the PolarFire SoC Video Kit:

```bash
wget https://github.com/polarfire-soc/meta-polarfire-soc-yocto-bsp/releases/download/v2023.02.1/core-image-minimal-dev-mpfs-video-kit-20230328105837.rootfs.wic.gz
```

Extract the archive to obtain the `.wic` file:

```bash
gunzip core-image-minimal-dev-mpfs-video-kit-20230328105837.rootfs.wic.gz
```

{: .warning }
Program the extracted `.wic` file, **not** the `.wic.gz` archive. Programming a compressed image can cause boot failures. Linux images using `.wic` or `.wic.gz` extensions use a GUID Partition Table (GPT) and are intended for the eMMC.

## eMMC content update procedure

1. Connect the USB-UART cable to **J12** on the board. This provides access to UART0 (HSS) and UART1 (Linux).
2. Open your serial terminal and connect at **115200 baud, 8 data bits, 1 stop bit, no parity, no flow control**.
3. Power on the board. The Microchip logo appears on UART0 as the HSS boots.
4. **Quickly** press any key in the terminal to interrupt the HSS boot. A `>>` prompt appears. You have a very brief window — if you miss it, power-cycle and try again.
5. At the `>>` prompt, type `mmc` to select eMMC as the boot source:

   ```text
   >> mmc
   ```

6. Then type `usbdmsc` to expose the eMMC as a USB mass storage device:

   ```text
   >> usbdmsc
   ```

   The terminal displays: `Waiting for USB Host to connect`.

7. Connect a USB cable to **J19** (USB OTG) on the board. The eMMC appears as a removable drive on your host PC.

8. Open **USBImager** on your host PC.

9. Select the extracted `.wic` image file in the **Image file** field.

10. Select the eMMC drive (the removable drive that appeared in step 7) in the **Device** field.

11. Click **Write** and wait for the process to complete.

12. Once writing is complete, unmount or eject the drive from your host PC.

13. Press **Ctrl+C** in the serial terminal to exit the `usbdmsc` command.

14. Disconnect the USB-OTG cable from J19.

15. Type `boot` to boot the newly written Linux image:

    ```text
    >> boot
    ```

16. HSS boot messages appear on UART0. Linux boot messages appear on UART1.

## Verifying the image

After the board boots, log in as `root` (no password required) on UART1 and confirm the kernel version:

```bash
uname -r
```

Expected output for the 2023.02.1 release:

```text
5.15.18-linux4microchip+fpga
```

## Next steps

After flashing the Linux image, return to the [Quickstart]({% link hardware/polarfire-soc-video-kit/quickstart.md %}) guide and continue from Step 5 to install the VectorBlox demo.
