---
title: Home
layout: home
nav_order: 1
---

# VectorBlox Documentation

VectorBlox is an SDK from Microchip for compiling and running quantized TFLite INT8 neural networks on the CoreVectorBlox accelerator running on PolarFire and PolarFire SoC FPGAs. This site collects documentation for the full VectorBlox stack: the SDK, the compression library, and the two reference video demos.

## The VectorBlox flow

1. Start from a model in ONNX, TensorFlow, PyTorch, OpenVINO, or Darknet.
2. Convert and quantize to a TFLite INT8 model using the SDK toolchain.
3. Compile the TFLite model into a `.vnnx` binary with `vnnx_compile` for a chosen hardware size (V250, V500, V1000) and compression mode (ncomp, comp, ucomp).
4. Validate the binary on the host simulator (C or Python).
5. Deploy the binary to a PolarFire (SoC) Video Kit and run it in the demo framework.

## Where to start

| You are... | Go to |
|---|---|
| A new user who wants a model running on the simulator | [Getting Started]({% link getting-started/index.md %}) |
| A model developer tuning conversion, quantization, or postprocessing | [SDK Overview]({% link sdk/index.md %}) |
| Trying a pretrained tutorial | [Tutorials]({% link tutorials/index.md %}) |
| Compressing a PyTorch model | [Compression Library]({% link compression_lib.md %}) |
| Bringing up a PolarFire (SoC) Video Kit | [Hardware]({% link hw_setup.md %}) |

## Source repositories

- [VectorBlox SDK](https://github.com/Microchip-Vectorblox/VectorBlox-SDK)
- [VectorBlox Compression](https://github.com/Microchip-Vectorblox/VectorBlox-Compression)
- [VectorBlox SoC Video Kit Demo](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo)
- [VectorBlox Video Kit Demo](https://github.com/Microchip-Vectorblox/VectorBlox-Video-Kit-Demo)
