---
title: Tutorials
layout: default
nav_order: 4
has_children: true
permalink: /tutorials/
---

# Tutorials

The SDK ships end-to-end tutorials that generate a `.vnnx` binary for a specific model. Each tutorial is a shell script under `$VBX_SDK/tutorials/<SOURCE>/<MODEL>/`. The script does all of the following:

1. Downloads the source model.
2. Converts it to TFLite (via `openvino2tensorflow`, `onnx2tf`, or `tflite_quantize`) if it is not already.
3. Adds a preprocessing layer with `tflite_preprocess`.
4. Compiles a binary with `vnnx_compile`.
5. Runs the binary through the Python simulator with a sample image.

Tutorials are grouped by model source. Run one with:

```bash
(vbx_env) ~/VectorBlox-SDK/tutorials/SOURCE_NAME/MODEL_NAME$ bash MODEL_NAME.sh
```

## Section index

| Page | Purpose |
|---|---|
| [Walkthrough: yolov5n]({% link tutorials/walkthrough-yolov5n.md %}) | Line-by-line reading of the `yolov5n_512x288` script |
| [Deploying a Tutorial Binary]({% link tutorials/deploy-to-soc.md %}) | Copy and run a compiled binary on the PolarFire SoC Video Kit |
| [Model Index by Source]({% link tutorials/model-index.md %}) | Directory of every tutorial grouped by source framework |
| [Metrics]({% link tutorials_metrics.md %}) | Runtime and accuracy for every included tutorial, split by compression mode |

---

Source: [VectorBlox-SDK/tutorials/README.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/tutorials/README.md)
