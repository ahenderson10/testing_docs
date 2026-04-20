---
title: MIPI Camera Bring-Up
layout: default
parent: PolarFire SoC Video Kit
grand_parent: Hardware
nav_order: 5
---

# MIPI Camera Bring-Up

The PolarFire SoC Video Kit supports a 4K dual-sensor MIPI camera daughter card. This page covers attaching the camera and resolving common bring-up issues.

{: .note }
HDMI input (J13) takes priority over the camera input. For the camera path to be active, the HDMI input cable must be unplugged.

## Prerequisites

- VectorBlox demo installed via the [quickstart script]({% link hardware/polarfire-soc-video-kit/quickstart.md %}) (`quick_start_3_0.sh` must have run successfully before attempting camera setup)
- Camera daughter card (dual sensor MIPI module)
- SSH or UART1 terminal access to the board

## Attaching the camera

1. Power off the board.
2. Plug the camera daughter card firmly into the **4K Camera MIPI** socket on the board.
3. Power on the board and wait for Linux to boot.

## Running the camera setup script

If the camera does not function correctly after the quickstart, run the one-time setup script:

```bash
cd ~/VectorBlox-SDK-release-v3.0/example/soc-video-c
bash setup_camera.sh
```

{: .note }
This script only needs to be run once. Run it only after the `quick_start_3_0.sh` script has completed successfully.

After running the setup script, power-cycle the board to enable automatic brightness adjustment.

## Starting the demo with the camera

From the `soc-video-c` example directory, build and launch the demo:

```bash
cd ~/VectorBlox-SDK-release-v3.0/example/soc-video-c
make overlay   # Adds the VectorBlox instance to the device tree (skip if setup_camera.sh ran successfully)
make
./run-video-model
```

{: .tip }
The `make overlay` step is not required if the camera setup script ran correctly and the overlay is already applied.

## Troubleshooting automatic brightness

If the camera feed does not automatically adjust brightness:

1. Verify the `make overlay` step has been run:

   ```bash
   cd ~/VectorBlox-SDK-release-v3.0/example/soc-video-c
   make overlay
   ```

2. Run the `auto_gain` executable:

   ```bash
   cd /opt/microchip
   ./auto_gain &
   ```

## Input source priority

The `v4l2-start_service.sh` startup service runs automatically at Linux boot and manages which input source is active:

| Condition | Active input |
|---|---|
| HDMI cable plugged into J13 | HDMI input (always takes priority) |
| HDMI cable unplugged from J13 | MIPI camera |

To switch from HDMI to camera input, unplug the HDMI cable from J13 before or after booting.
