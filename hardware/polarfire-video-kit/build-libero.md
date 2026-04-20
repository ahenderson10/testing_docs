---
title: Build with Libero
layout: default
parent: PolarFire Video Kit
grand_parent: Hardware
nav_order: 2
---

# Building the PolarFire Video Kit Design with Libero

This page explains how to regenerate the VectorBlox FPGA design from the Tcl script using Libero SoC v2024.2. Follow this path if you want to modify the design, incorporate custom neural network binaries, or produce your own `.job` file.

## Prerequisites

| Requirement | Detail |
|---|---|
| Libero SoC | Version **2024.2** ([download](https://www.microsemi.com/product-directory/design-resources/1750-libero-soc#downloads)) |
| Libero SoC Silver License | Free; see [Microchip licensing](https://www.microchip.com/en-us/products/fpgas-and-plds/fpga-and-soc-design-tools/fpga/licensing) |
| Libero SoC VectorBlox License | Free; same licensing page |
| SoftConsole | Required on native OS for building the MiV bare-metal application |
| SmartHLS | Version 2024.2 (`shls` must be in your `$PATH` on Linux) |
| `riscv64-unknown-elf-gcc` | From SoftConsole; must be in `$PATH` on Linux |
| VectorBlox SDK | [release-v2.0.3](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/release-v2.0.3) |

{: .important }
Both licenses are free of charge but must be requested from Microchip before Libero will run synthesis or place-and-route.

## Preparing the neural network hex files

Before running the Tcl script, the neural network models and SPI flash configuration must be generated:

```bash
# Activate the VectorBlox SDK virtual environment
source $VBX_SDK/setup_vars.sh

# Download pre-generated hex files (contains V1000 ncomp model binaries)
# Extract into the root of the VectorBlox-Video-Kit-Demo repo
```

Download [hex_V1000_2.0.3.zip](https://github.com/Microchip-Vectorblox/assets/releases/download/assets/hex_V1000_2.0.3.zip) and extract the files into the repository root directory.

## Building on Windows

1. Clone the [VectorBlox-Video-Kit-Demo](https://github.com/Microchip-Vectorblox/VectorBlox-Video-Kit-Demo) repository into a Windows-accessible directory.
2. Extract `hex_V1000_2.0.3.zip` into the same directory.
3. Open SoftConsole and import the project (see Section 6 of the Demo Guide PDF in `docs/`).
4. Set the **Build Configuration** to `Release` and build the project.
5. Open Libero SoC v2024.2.
6. Press **Ctrl+U** to open the Execute Script dialog.
7. Select `MPF_VIDEO_KIT_VECTORBLOX_DESIGN.tcl` and add the argument `GENERATE_PROGRAMMING_DATA`.
8. Click **Run**.
9. Program the device via Libero (see Section 5 of the Demo Guide PDF).

## Building on Linux

Ensure `riscv64-unknown-elf-gcc`, `libero`, and `shls` (SmartHLS 2024.2) are on your `$PATH`:

```bash
export PATH=/path/to/softconsole/riscv64/bin:/path/to/libero/bin:/path/to/smartHLS/bin:$PATH
```

Extract the hex files and run:

```bash
cd VectorBlox-Video-Kit-Demo
# Extract hex_V1000_2.0.3.zip here first
make bitstream
```

The `make bitstream` target builds the MiV application and then runs the Tcl script to generate the Libero project and produce a `.job` file.

## Running the Tcl script manually

If you prefer to drive Libero directly, open the Execute Script dialog (**Ctrl+U**) and run:

```tcl
MPF_VIDEO_KIT_VECTORBLOX_DESIGN.tcl
```

### Supported Tcl arguments

| Argument | Description |
|---|---|
| `SYNTHESIZE` | Run synthesis after design generation |
| `PLACEROUTE` | Run synthesis and place-and-route |
| `VERIFY_TIMING` | Run synthesis, place-and-route, and timing verification |
| `GENERATE_PROGRAMMING_DATA` | Generate files needed for bitstream export |
| `EXPORT_FPE` | Run full flow and export a FlashPro Express `.job` file |

Example — generate and export a job file in one step:

```bash
# Inside Libero's Execute Script dialog, set Arguments to:
EXPORT_FPE
```

## Programming after building

Double-clicking **Run Program Action** in Libero's design flow panel will run all prerequisite steps (synthesize, place-and-route, timing) and then program the connected device.

Alternatively, use the exported `.job` file in **FlashPro Express** (see [Board Bring-Up]({% link hardware/polarfire-video-kit/bring-up.md %})).
