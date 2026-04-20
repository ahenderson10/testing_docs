---
title: PolarFire SoC Video Kit
layout: default
parent: Hardware
nav_order: 2
has_children: true
permalink: /hardware/polarfire-soc-video-kit/
---

# PolarFire SoC Video Kit

The PolarFire SoC Video Kit runs the VectorBlox demo under full **Yocto Linux** on the MPFS250T's U54 application cores. This gives you SSH access, a standard file system, and the ability to swap neural network binaries at runtime without reflashing the FPGA.

## Overview

| Item | Detail |
|---|---|
| FPGA | PolarFire SoC MPFS250T |
| Libero SoC version | 2025.1 |
| TCL script | `MPFS_VIDEO_KIT_REFERENCE_DESIGN.tcl` |
| Operating system | Yocto Linux 2023.02.1 |
| Release job file | Available on the [GitHub releases page](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo/releases) |
| Default VectorBlox accelerator | V1000 (no compression) |
| Compression options | Standard (`COMP`), unstructured (`UCOMP`) via alternate job files |
| Built-in demos | Classification, Object Detection, Face Recognition, Pose Estimation |

## Sub-pages

| Page | Contents |
|---|---|
| [Quickstart]({% link hardware/polarfire-soc-video-kit/quickstart.md %}) | Cabling, FlashPro programming, and the quickstart script |
| [Jumper Settings]({% link hardware/polarfire-soc-video-kit/jumper-settings.md %}) | Required jumper positions with a labeled board diagram |
| [Build with Libero]({% link hardware/polarfire-soc-video-kit/build-libero.md %}) | Licensing and the Tcl build flow for regenerating the design from source |
| [Flash Yocto Linux]({% link hardware/polarfire-soc-video-kit/flash-yocto.md %}) | Updating the eMMC with a new Yocto image via USBImager and the HSS |
| [MIPI Camera Bring-Up]({% link hardware/polarfire-soc-video-kit/mipi-camera.md %}) | Setting up and troubleshooting the 4K MIPI camera daughter card |

## Choosing a job file

Three job file variants are available from the releases page:

| Variant | Filename suffix | Description |
|---|---|---|
| Default | (no suffix) | V1000 accelerator, no compression |
| Compression | `COMP` | V1000 with structured 2:4 compression support |
| Unstructured compression | `UCOMP` | V1000 with unstructured compression support (HDMI or MIPI, not both simultaneously) |

{: .note }
If you switch between job file variants, delete any existing `VectorBlox-SDK-release` folders on the board before re-running the quickstart script.
