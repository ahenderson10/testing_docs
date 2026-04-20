---
title: Inference & Simulation
layout: default
parent: SDK Overview
nav_order: 4
---

# Inference and Simulation

After `vnnx_compile` produces a `.vnnx` binary, validate it on the bit-accurate host simulator before moving to hardware. The SDK ships both a C simulator (`sim-run-model`) and Python inference scripts for common model families.

Refer to [Toolchain & Conversion]({% link sdk/toolchain.md %}) and [Graph Compilation]({% link sdk/compilation.md %}) for the steps that produce the binary.

## C simulator

Runs a model against an image (or the embedded test data if no image is given) and prints the checksum. Postprocessing type is optional — see [Postprocessing Functions]({% link sdk/postprocessing.md %}) for valid values.

```bash
$VBX_SDK/example/sim-c/sim-run-model MODEL_NAME.vnnx IMAGE.jpg POSTPROCESSTYPE
```

## Python simulator

The Python scripts under `$VBX_SDK/example/python/` run the same graph with richer postprocessing and produce annotated output images. Exact arguments vary per model — the tutorials call each script with the correct flags for that model.

```bash
# Classifier
python $VBX_SDK/example/python/classifier.py MODEL_NAME.vnnx IMAGE.jpg

# YOLOv5n
python $VBX_SDK/example/python/yoloInfer.py yolov5n_V1000_ncomp.vnnx IMAGE.jpg \
  -j yolov5n.json -v 5 -l coco.names -t 0.25

# YOLOv8n
python $VBX_SDK/example/python/yoloInfer.py yolov8n_V1000_ncomp.vnnx IMAGE.jpg \
  --v 8 -l coco.names

# Posenet
python $VBX_SDK/example/python/posenetInfer.py posenet_V1000_ncomp.vnnx -i IMAGE.jpg
```

---

Source: [VectorBlox-SDK/docs/inference_simulation.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/inference_simulation.md)
