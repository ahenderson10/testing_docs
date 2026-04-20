---
title: Hardware
layout: default
nav_order: 3
has_children: true
permalink: /hardware/
---

# Hardware

VectorBlox runs on two PolarFire FPGA evaluation boards. Both boards ship a pre-built `.job` file so you can be running neural networks within minutes, or you can rebuild the FPGA design from source using Libero SoC.

## Kit comparison

| Feature | PolarFire Video Kit | PolarFire SoC Video Kit |
|---|---|---|
| FPGA device | PolarFire MPF300T | PolarFire SoC MPFS250T |
| Libero version | 2024.2 | 2025.1 |
| Operating system | Bare-metal (MiV soft core) | Yocto Linux (2023.02.1) |
| VectorBlox accelerator size | V1000 | V1000 |
| Video input | HDMI (J35) | HDMI (J13) or MIPI camera |
| Video output | HDMI (J2) | HDMI (J14) |
| Demo control | `SW1` / `SW2` push-buttons | Serial terminal (UART1) or SSH |
| Compression support | Standard (ncomp) | ncomp, comp, ucomp |

Both kits include classification (MobileNetV2) and object detection (YOLOv8) demos out of the box.

## Which kit do you have?

- **PolarFire Video Kit** — rectangular board with an HDMI input and output, two red push-buttons. Uses a MiV RISC-V soft processor. Go to [PolarFire Video Kit]({% link hardware/polarfire-video-kit.md %}).
- **PolarFire SoC Video Kit** — larger board with four USB connectors (J5, J12, J19), a 4K MIPI camera socket, and two HDMI ports (J13, J14). Runs full Yocto Linux on the U54 application cores. Go to [PolarFire SoC Video Kit]({% link hardware/polarfire-soc-video-kit.md %}).

## Prerequisites for both kits

- **FlashPro Express** — programs the FPGA bitstream (`.job` file). Available as a standalone download or as part of Libero SoC from [Microchip's download page](https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug).
- A USB-A to Micro-USB cable for the FlashPro6 port.
- An HDMI monitor and cable for the video output.
