---
title: Run Your First Tutorial
layout: default
parent: Getting Started
nav_order: 3
---

# Run Your First Tutorial

Each tutorial is a self-contained shell script that downloads a model, converts it to TFLite INT8, compiles it into a `.vnnx` binary, and runs it through the simulator with a sample image. Tutorials live under `$VBX_SDK/tutorials/<SOURCE>/<MODEL>/`.

## Run the script

```bash
cd $VBX_SDK/tutorials/SOURCE_NAME/MODEL_NAME
bash MODEL_NAME.sh
```

For example, to run the ONNX SqueezeNet tutorial:

```bash
cd $VBX_SDK/tutorials/onnx/onnx_squeezenet1.1
bash onnx_squeezenet1.1.sh
```

## What the script does

1. Downloads the source model.
2. Converts it to TFLite INT8 if it is not already in that format (using `openvino2tensorflow`, `onnx2tf`, or `tflite_quantize`).
3. Adds a preprocessing layer with `tflite_preprocess` so the model accepts `uint8` inputs.
4. Calls `vnnx_compile` to generate the `.vnnx` (and `.hex` / `.ucomp`) binary.
5. Runs the binary through the Python simulator with a sample image and prints the result.

## Next steps

- Walk through a tutorial line-by-line in [Walkthrough: yolov5n]({% link tutorials/walkthrough-yolov5n.md %}).
- Deploy the resulting `.vnnx` to a Video Kit in [Deploying a Tutorial Binary]({% link tutorials/deploy-to-soc.md %}).
- Dig into the underlying tooling in [SDK Overview]({% link sdk/index.md %}).

---

Source: [VectorBlox-SDK/README.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/README.md), [tutorial_overview.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/tutorials/README.md)
