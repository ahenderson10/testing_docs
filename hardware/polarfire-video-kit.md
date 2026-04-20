---
title: PolarFire Video Kit
layout: default
parent: Hardware
nav_order: 1
has_children: true
permalink: /hardware/polarfire-video-kit/
---

# PolarFire Video Kit

The PolarFire Video Kit runs the VectorBlox demo on a bare-metal MiV RISC-V soft processor. Because there is no operating system, the neural network binaries and SPI flash configuration are baked into the FPGA bitstream via a `.job` file.

## Overview

| Item | Detail |
|---|---|
| FPGA | PolarFire MPF300T (1FCG1152E) |
| Libero SoC version | 2024.2 |
| TCL script | `MPF_VIDEO_KIT_VECTORBLOX_DESIGN.tcl` |
| Release job file | Available on the [GitHub releases page](https://github.com/Microchip-Vectorblox/VectorBlox-Video-Kit-Demo/releases) |
| Required SDK version | [release-v2.0.3](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/release-v2.0.3) |
| Built-in demos | Classification (MobileNetV2), Object Detection (YOLOv8) |

## Quick start — use the pre-built job file

The fastest way to get the demo running is to program the board with the pre-built `.job` file:

1. Download the latest `.zip` from the [releases page](https://github.com/Microchip-Vectorblox/VectorBlox-Video-Kit-Demo/releases) and extract it.
2. Connect a Micro-USB cable from your PC to the **FlashPro6** port on the board.
3. Connect HDMI input to **J35** and HDMI output to **J2**.
4. Power on the board.
5. Open FlashPro Express, create a new project, and load the `.job` file.
6. Click **Run**. Programming takes approximately two minutes.
7. Power-cycle the board. The demo launches automatically.

## Controlling the demo

| Button | Action |
|---|---|
| `SW1` | Toggle the model-selection menu |
| `SW2` | Scroll through available models |
| `SW1` (again) | Load the highlighted model |

The demo cycles between a Classification mode (MobileNetV2) and an Object Detection mode (YOLOv8).

## Sub-pages

- [Board Bring-Up]({% link hardware/polarfire-video-kit/bring-up.md %}) — cabling, power, HDMI, and MIPI connections.
- [Build with Libero]({% link hardware/polarfire-video-kit/build-libero.md %}) — Tcl build flow for regenerating the FPGA design from source.
