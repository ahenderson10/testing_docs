---
title: Build with Libero
layout: default
parent: PolarFire SoC Video Kit
grand_parent: Hardware
nav_order: 2
---

# Building the PolarFire SoC Video Kit Design with Libero

This page explains how to regenerate the VectorBlox reference design from the Tcl script using Libero SoC v2025.1. Follow this path when you want to modify the design or produce a custom `.job` file.

## Prerequisites

| Requirement | Detail |
|---|---|
| Libero SoC | Version **2025.1** ([download](https://www.microsemi.com/product-directory/design-resources/1750-libero-soc#downloads)) |
| Libero SoC Silver License | Free — [Microchip licensing page](https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/fpga/licensing) |
| Libero SoC VectorBlox License | Free — same licensing page |
| `wget` | Required for the `HSS_UPDATE` argument. Pre-installed on most Linux systems; on Windows install version 1.14 or later and add it to your `PATH` |

{: .important }
Both the Silver and VectorBlox licenses are free of charge. Request them from Microchip before starting synthesis; Libero will refuse to run place-and-route without valid licenses.

## Standard design generation

1. Clone or download the [VectorBlox-SoC-Video-Kit-Demo](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo) repository.
2. Open Libero SoC v2025.1.
3. Press **Ctrl+U** to open the Execute Script dialog.
4. Select `MPFS_VIDEO_KIT_REFERENCE_DESIGN.tcl`.
5. Add any desired arguments (see tables below).
6. Click **Run**.
7. Once design generation completes, run the Libero SoC design flow (double-click a flow stage in the left panel).

## Supported Tcl arguments

### Flow-control arguments

| Argument | Description |
|---|---|
| `HSS_UPDATE` | Downloads the HSS release hex file for this design version and adds it as an eNVM client. Requires `wget`. |
| `SYNTHESIZE` | Runs synthesis after design generation |
| `PLACEROUTE` | Runs synthesis and place-and-route |
| `VERIFY_TIMING` | Runs synthesis, place-and-route, and timing verification |
| `GENERATE_PROGRAMMING_DATA` | Generates the files needed to export a bitstream |
| `EXPORT_FPE` | Runs the full flow and exports a FlashPro Express `.job` file to the local directory |

### Compression arguments

By default the project is generated with both HDMI and MIPI inputs and the VectorBlox IP configured with **no compression** support.

| Argument | Description |
|---|---|
| `COMP` | Enables structured compression support (both HDMI and MIPI inputs) |
| `UCOMP` | Enables unstructured compression support (HDMI **or** MIPI, not both simultaneously) |

### Input arguments

| Argument | Description |
|---|---|
| `HDMI` | Generates the project with HDMI input only |
| `MIPI` | Generates the project with MIPI camera input only |

## Programming the FPGA

Double-clicking **Run Program Action** in Libero's design flow panel runs all required prerequisite steps and then programs the connected device.

Alternatively, export a `.job` file using the `EXPORT_FPE` argument and program it via FlashPro Express (see [Quickstart — Step 3]({% link hardware/polarfire-soc-video-kit/quickstart.md %})).

## Next steps

After programming, complete the board setup in the [Quickstart]({% link hardware/polarfire-soc-video-kit/quickstart.md %}) guide. At Step 3 of the quickstart, use the `.job` file exported from Libero rather than the pre-built release file.
