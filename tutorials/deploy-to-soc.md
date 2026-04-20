---
title: Deploying a Tutorial Binary
layout: default
parent: Tutorials
nav_order: 2
---

# Deploying a Tutorial Binary to the PolarFire SoC Video Kit

Before continuing, make sure the hardware is set up — see [Hardware]({% link hw_setup.md %}) for the PolarFire SoC Video Kit quickstart, including how to find the board's IP address.

This page uses `yolov5n_512x288_V1000_ncomp.vnnx` from the [YOLOv5n walkthrough]({% link tutorials/walkthrough-yolov5n.md %}) as the running example. Replace it with whatever binary you produced.

## 1. Transfer the binary to the board

Confirm the host can reach the board:

```bash
ping <SOC_IP>
```

`<SOC_IP>` is the IP address of the Video Kit (see the hardware setup guide). Copy the tutorial binary and a test image over SCP:

```bash
scp yolov5n_512x288_V1000_ncomp.vnnx root@<SOC_IP>:~/samples_V1000_NCOMP_3.0/
scp $VBX_SDK/tutorials/test_images/dog.jpg root@<SOC_IP>:~
```

## 2. Run the binary on the board and cross-check with the simulator

SSH in:

```bash
ssh root@<SOC_IP>
```

Build and run the `soc-c` single-model harness:

```bash
cd VectorBlox-SDK-release-v3.0/example/soc-c
make
./run-model ~/samples_V1000_NCOMP_3.0/yolov5n_512x288_V1000_ncomp.vnnx ~/dog.jpg YOLOV5
```

The hardware output (checksums and detection results) should match the C simulator output from the walkthrough. If it does not, the binary is not bit-accurate — re-check your compile flags.

## 3. Add the binary to the video demo

```bash
cd ~/VectorBlox-SDK-release-v3.0/example/soc-video-c
make clean
```

Open `demo_models.h` and add your binary to the `model_descr_t models[]` array. You can do this manually or with the one-liner below, which inserts the new entry after the Midas V2 row:

```bash
grep -rF "yolov5n_512x288_V1000_ncomp" demo_models.h || \
sed -i '\|{"Midas V2", "/home/root/samples_V1000_NCOMP_3.0/Midas-V2-Quantized_V1000_ncomp.vnnx", 0, "PIXEL"},|a {"Yolov5n", "/home/root/samples_V1000_NCOMP_3.0/yolov5n_512x288_V1000_ncomp.vnnx", 0, "YOLOV5"},' demo_models.h
```

If you run the `sed` command, copy it from a browser and paste into your terminal rather than typing it by hand.

Verify the change:

```bash
cat demo_models.h
```

## 4. Rebuild and launch the demo

```bash
make
./run-video-demo
```

## 5. Exit the demo

- Type `q` followed by <kbd>Enter</kbd> inside the running demo.
- Type `exit` to close the SSH session and return to your SDK host.

---

Sources: [VectorBlox-SDK/example/soc-c/README.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/example/soc-c/README.md), [VectorBlox-SoC-Video-Kit-Demo/docs/adding_models.md](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo/blob/main/docs/adding_models.md)
