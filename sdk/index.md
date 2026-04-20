---
title: SDK Overview
layout: default
nav_order: 4
has_children: true
permalink: /sdk/
---

# SDK Overview

The VectorBlox SDK turns a trained floating-point model into a quantized `.vnnx` binary that can run on a CoreVectorBlox accelerator (or on the bundled host simulator). The flow is:

```text
source model ──▶ TFLite INT8 ──▶ preprocessed TFLite ──▶ .vnnx binary ──▶ simulator or hardware
   (ONNX,          (via              (tflite_preprocess)   (vnnx_compile,
    TF, etc.)     onnx2tf,                                  V250/V500/V1000,
                  openvino2tensorflow,                      ncomp/comp/ucomp)
                  tflite_quantize)
```

## Section index

| Page | Covers |
|---|---|
| [Toolchain & Conversion]({% link sdk/toolchain.md %}) | `generate_npy`, `openvino2tensorflow`, `onnx2tf`, `tflite_quantize`, `tflite_cut`, `tflite_preprocess`, `tflite_postprocess`, `omz_downloader`, `mo` |
| [Graph Compilation]({% link sdk/compilation.md %}) | `vnnx_compile` flags, size configs (V250 / V500 / V1000), compression modes (ncomp / comp / ucomp) |
| [Supported Operators]({% link sdk/operators.md %}) | Full table of 74 supported TFLite INT8 operators and their parameter constraints |
| [Inference & Simulation]({% link sdk/simulation.md %}) | `sim-run-model`, `classifier.py`, `yoloInfer.py`, `posenetInfer.py` |
| [C API Reference]({% link sdk/c-api.md %}) | Enum types, model query functions, `vbx_cnn_*` runtime calls, Python simulator API |
| [Postprocessing Functions]({% link sdk/postprocessing.md %}) | `post_process_yolo_int8`, `post_process_ultra_int8`, `decodeMultiplePoses_int8`, `post_process_scrfd_int8`, `post_process_ssd_torch_int8`, NMS helpers |
| [Resource Utilization]({% link sdk/resource-utilization.md %}) | FPGA fabric / LSRAM / uSRAM / MATH usage by size and compression configuration |

---

Source: [VectorBlox-SDK/docs/](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/master/docs)
