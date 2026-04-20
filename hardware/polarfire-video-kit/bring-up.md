---
title: Board Bring-Up
layout: default
parent: PolarFire Video Kit
grand_parent: Hardware
nav_order: 1
---

# PolarFire Video Kit Board Bring-Up

This page covers the physical connections and power-on sequence needed to run the VectorBlox video demo on the PolarFire Video Kit.

## Prerequisites

- PolarFire Video Kit board
- Micro-USB cable (for FlashPro6)
- Two HDMI cables (input source and output monitor)
- 12 V power supply
- FlashPro Express installed ([download here](https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/programming-and-debug))

## Cable connections

| Port | Cable type | Purpose |
|---|---|---|
| **J35** | HDMI | Video input (connect to HDMI source, e.g., laptop, camera) |
| **J2** | HDMI | Video output (connect to monitor or display) |
| **FlashPro6** | Micro-USB | FPGA programming via FlashPro Express |
| Power jack | 12 V barrel connector | Board power |

{: .note }
The board must be powered on **before** FlashPro Express attempts to connect. If FlashPro Express does not detect the board, cycle power and retry.

## Power-on sequence

1. Connect all cables (HDMI input, HDMI output, FlashPro6 USB) before powering on.
2. Connect the 12 V power supply to the power jack.
3. The board initialises and, if a valid bitstream is already programmed, the demo launches within a few seconds.

## Programming the board

1. Download the latest `.zip` release from [GitHub](https://github.com/Microchip-Vectorblox/VectorBlox-Video-Kit-Demo/releases) and extract the `.job` file.
2. Open FlashPro Express.
3. Select **Project → New Job Project** and browse to the extracted `.job` file.
4. Click **Run**. Wait for the status to read **PASSED**.
5. Power-cycle the board to boot into the new bitstream.

## Verifying the demo

After power-cycling, within a few seconds you should see:

- The HDMI output monitor shows a live video feed with an overlaid classification or detection result.
- Pressing `SW1` opens the model selection menu; `SW2` scrolls models; pressing `SW1` again loads the selected model.

{: .tip }
If the HDMI output is blank, verify that the input source on J35 is active and that the output monitor is set to the correct input channel.

## Next steps

To rebuild the FPGA design from source (for example, to add custom models), see [Build with Libero]({% link hardware/polarfire-video-kit/build-libero.md %}).
