---
title: Quickstart
layout: default
parent: PolarFire SoC Video Kit
grand_parent: Hardware
nav_order: 1
---

# PolarFire SoC Video Kit Quickstart

This guide takes you from an unpowered board to a running VectorBlox demo in five steps.

## Table of contents

- [Prerequisites](#prerequisites)
- [Step 1: Jumper configuration](#step-1-jumper-configuration)
- [Step 2: Hardware connections](#step-2-hardware-connections)
- [Step 3: Program the job file](#step-3-program-the-job-file)
- [Step 4: Program the Linux image](#step-4-program-the-linux-image)
- [Step 5: Set up the VectorBlox demo](#step-5-set-up-the-vectorblox-demo)
- [Controlling the demo](#controlling-the-demo)

## Prerequisites

- **FlashPro Express** — programs the FPGA bitstream. Available standalone or as part of Libero SoC from [Microchip's download page](https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug).
- **USBImager** — programs the Yocto Linux image to the eMMC. Download from [bztsrc.gitlab.io/usbimager](https://bztsrc.gitlab.io/usbimager/).
- A serial terminal application (115200 baud, 8N1, no flow control). See [Microchip's serial terminal setup guide](https://onlinedocs.microchip.com/oxy/GUID-E89F0380-CE10-4E39-B622-CA56F677F477-en-US-3/GUID-252CFF5A-1DB8-421F-B210-A5C575B68FE7.html) for details.

## Step 1: Jumper configuration

The board ships with jumpers in the correct positions. If you have changed them or are troubleshooting, confirm the required settings on the [Jumper Settings]({% link hardware/polarfire-soc-video-kit/jumper-settings.md %}) page.

{: .important }
Jumpers **J16** and **J35** must **not** be open. Both must be connected to pins 2 and 3.

## Step 2: Hardware connections

Connect the following before powering on. Refer to the labeled board diagram below for port locations.

![PolarFire SoC Video Kit connections]({{ "/assets/images/hardware-connections.png" | relative_url }})

| Port | Type | Description |
|---|---|---|
| 4K Camera MIPI | Camera | Dual camera sensor module (optional; HDMI takes priority if plugged in) |
| **J13** HDMI 1xRX | HDMI | Connect to an HDMI source on the host PC for video input |
| **J14** HDMI 1xTX | HDMI | Connect to an output monitor for video display |
| Dual Gigabit Ethernet | Ethernet | Used for SSH and downloading models; either port works |
| **J39** | Power | 12 V, 5 A power adapter |
| **J12** | Micro-USB | UART port — UART0 connects to the HSS; UART1 connects to the Linux terminal |
| **J5** | Micro-USB | FlashPro6 port — used to program the job file |
| **J19** | Micro-USB | USB OTG port — used to transfer the Yocto image via USBImager |

## Step 3: Program the job file

Programming the job file writes the FPGA bitstream to the fabric and loads the Hart Software Services (HSS) payload into eNVM.

1. Ensure cables for **J5** (FlashPro6) and **J12** (UART) are both connected to your PC.
2. Set up a serial terminal on the UART port (115200 baud, 8N1).
3. Download the latest release `.zip` from [GitHub releases](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo/releases) and extract it.
4. Open FlashPro Express, create a **New Job Project**, and browse to the extracted `.job` file.
5. Click **Run** and wait for the status to show **PASSED**.
6. Power-cycle the board once programming is complete.

{: .tip }
The `COMP` and `UCOMP` job file variants enable compression support. Use the default job file unless you specifically need compressed model inference. If switching variants, delete any existing `VectorBlox-SDK-release` folders on the board first.

## Step 4: Program the Linux image

The demo runs on Yocto Linux **2023.02.1**, which should be pre-installed. If you need to reflash it:

1. Download the Yocto image:

   ```bash
   wget https://github.com/polarfire-soc/meta-polarfire-soc-yocto-bsp/releases/download/v2023.02.1/core-image-minimal-dev-mpfs-video-kit-20230328105837.rootfs.wic.gz
   ```

2. Extract the `.wic.gz` file to obtain the `.wic` image file.

   {: .warning }
   Program the extracted `.wic` file — do not program the `.wic.gz` directly. Compressed images can cause boot failures.

3. Follow the full eMMC flashing procedure in [Flash Yocto Linux]({% link hardware/polarfire-soc-video-kit/flash-yocto.md %}).

4. To verify the Linux version after booting, log in as `root` (no password) on UART1 and run:

   ```bash
   uname -r
   ```

## Step 5: Set up the VectorBlox demo

1. Log in as `root` via UART1 or SSH. To find the board's IP address:

   ```bash
   ip a | grep dynamic
   ```

   Or use Ethernet and check your router's DHCP table.

2. Ensure both HDMI cables (J13 and J14) are connected. If you see noise or a white screen on the output, check the cables and power-cycle the board.

3. If using the camera, plug the camera daughter card into the MIPI socket.

4. Download and run the quickstart script:

   ```bash
   wget --no-check-certificate https://raw.githubusercontent.com/Microchip-Vectorblox/assets/refs/heads/main/quick_start_3_0.sh
   ```

   For the **default** job file (no compression):

   ```bash
   bash quick_start_3_0.sh
   ```

   For the **COMP** job file:

   ```bash
   bash quick_start_3_0.sh 3.0 COMP
   ```

   For the **UCOMP** job file:

   ```bash
   bash quick_start_3_0.sh 3.0 UCOMP
   ```

5. Once the script finishes, wait approximately 30 seconds, then power-cycle the board to allow file transfers to complete.

{: .note }
The `v4l2-start_service.sh` script runs automatically at Linux boot. HDMI input has priority over the camera input — unplug the HDMI input (J13) to switch to the camera path.

## Controlling the demo

| Key | Action |
|---|---|
| `Enter` | Switch to the next model |
| `q` then `Enter` | Quit the demo |
| `a` (Recognition mode) | Highlight the largest face; press `a` again to add it to the database |
| `d` (Recognition mode) | List face embeddings by index; enter an index to delete that entry |
| `b` (Pose Estimation models) | Toggle blackout options for the image output |

Sample face video for testing Face Recognition mode: [SampleFaces.mp4](https://github.com/Microchip-Vectorblox/assets/releases/download/assets/SampleFaces.mp4).

## Next steps

- To rebuild the FPGA design from source, see [Build with Libero]({% link hardware/polarfire-soc-video-kit/build-libero.md %}).
- To reflash the Yocto Linux image, see [Flash Yocto Linux]({% link hardware/polarfire-soc-video-kit/flash-yocto.md %}).
- To configure the MIPI camera, see [MIPI Camera Bring-Up]({% link hardware/polarfire-soc-video-kit/mipi-camera.md %}).
