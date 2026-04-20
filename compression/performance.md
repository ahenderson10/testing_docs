---
title: Performance
layout: default
parent: Compression Library
nav_order: 3
---

# Compression Performance

This page presents accuracy and efficiency benchmarks for VectorBlox-compressed models. All figures are from the VectorBlox-Compression repository.

Column definitions:

| Column | Meaning |
|---|---|
| **Accuracy** | COCO mAP50–95 (object detection) or ImageNet Top-1 accuracy (classification) |
| **MMult** | Millions of multiply operations at inference |
| **MParams** | Millions of non-zero parameters |
| **ΔAcc** | Accuracy change versus the uncompressed baseline |
| **Mult Ratio** | Reduction in multiply operations versus baseline |

## COCO object detection

### Structured compression

| Model | Accuracy | MMult | MParams | ΔAcc | Mult Ratio |
|---|---:|---:|---:|---:|---:|
| yolov8n | 37.36 | 4386 | 3.160 | — | — |
| yolov8n_comp66 | 36.26 | 1659 | 1.095 | −1.10 | 2.64× |
| yolov8s | 44.97 | 14328 | 11.165 | — | — |
| yolov8s_comp68 | 43.75 | 5153 | 3.629 | −1.22 | 2.78× |
| yolov8m | 50.33 | 39516 | 25.895 | — | — |
| yolov8m_comp69 | 49.30 | 14264 | 8.090 | −1.03 | 2.77× |
| yolov9t | 38.35 | 4134 | 2.102 | — | — |
| yolov9t_comp61 | 37.20 | 1669 | 0.830 | −1.15 | 2.48× |
| yolov9s | 46.54 | 13485 | 7.207 | — | — |
| yolov9s_comp68 | 45.72 | 5014 | 2.295 | −0.82 | 2.69× |
| yolov9m | 51.53 | 38477 | 20.079 | — | — |
| yolov9m_comp67 | 50.49 | 14112 | 6.546 | −1.04 | 2.73× |
| yolo11n | 39.38 | 3288 | 2.650 | — | — |
| yolo11n_comp63 | 38.05 | 1374 | 0.981 | −1.33 | 2.39× |
| yolo11s | 46.92 | 10826 | 9.452 | — | — |
| yolo11s_comp68 | 45.51 | 4052 | 3.028 | −1.41 | 2.67× |
| yolo11m | 51.53 | 34120 | 20.100 | — | — |
| yolo11m_comp65 | 50.61 | 13310 | 7.048 | −0.92 | 2.56× |

### Unstructured compression

| Model | Accuracy | MMult | MParams | ΔAcc | Mult Ratio |
|---|---:|---:|---:|---:|---:|
| yolov8n | 37.36 | 4386 | 3.160 | — | — |
| yolov8n_ucomp70 | 36.57 | 1514 | 0.958 | −0.79 | 2.90× |
| yolov8n_ucomp75 | 36.18 | 1316 | 0.801 | −1.18 | 3.33× |
| yolov8n_ucomp80 | 35.93 | 1112 | 0.644 | −1.43 | 3.95× |
| yolov8n_ucomp85 | 34.76 | 898 | 0.486 | −2.60 | 4.88× |
| yolov8n_ucomp90 | 32.97 | 665 | 0.330 | −4.39 | 6.59× |
| yolov9t | 38.35 | 4134 | 2.102 | — | — |
| yolov9t_ucomp80 | 35.66 | 997 | 0.434 | −2.69 | 4.15× |
| yolov9s | 46.54 | 13485 | 7.207 | — | — |
| yolov9s_ucomp80 | 45.51 | 3377 | 1.460 | −1.03 | 3.99× |
| yolov9s_ucomp85 | 44.22 | 2677 | 1.101 | −2.32 | 5.04× |
| yolo11n | 39.38 | 3288 | 2.650 | — | — |
| yolo11n_ucomp75 | 38.21 | 1131 | 0.797 | −1.17 | 2.91× |
| yolo11n_ucomp80 | 37.96 | 1061 | 0.749 | −1.42 | 3.10× |

## ImageNet classification

### Structured compression

| Model | Accuracy | MMult | MParams | ΔAcc | Mult Ratio |
|---|---:|---:|---:|---:|---:|
| resnet18 | 70.42 | 1814 | 11.685 | — | — |
| resnet18_comp66 | 70.12 | 880 | 4.336 | −0.30 | 2.06× |
| resnet18_comp70 | 69.83 | 836 | 3.894 | −0.59 | 2.17× |
| mobilenet_v3_large | 75.27 | 217 | 5.471 | — | — |
| mobilenet_v3_large_comp56 | 72.89 | 107 | 3.883 | −2.38 | 2.03× |

### Unstructured compression

| Model | Accuracy | MMult | MParams | ΔAcc | Mult Ratio |
|---|---:|---:|---:|---:|---:|
| resnet18 | 70.42 | 1814 | 11.685 | — | — |
| resnet18_ucomp75 | 70.24 | 636 | 3.317 | −0.17 | 2.85× |
| resnet18_ucomp85 | 69.56 | 490 | 2.201 | −0.85 | 3.70× |
| resnet18_ucomp90 | 68.50 | 407 | 1.643 | −1.91 | 4.45× |
| resnet18_ucomp93 | 66.98 | 352 | 1.308 | −3.43 | 5.16× |
| mobilenet_v3_large | 75.27 | 217 | 5.471 | — | — |
| mobilenet_v3_large_ucomp80 | 73.34 | 85 | 3.190 | −1.93 | 2.57× |

## Key observations

- Structured compression at ~66–68% sparsity reduces multiply operations by **2.5–2.8×** with accuracy drops of **0.8–1.4 mAP** on COCO.
- Unstructured compression at 80% sparsity achieves **3.9–4.5×** multiply reduction with **1–2.7 mAP** accuracy drop.
- Weight pairing (included in both modes) provides a significant accuracy boost at no cost in compression ratio.
- Classification models (ResNet-18) tolerate very high unstructured sparsity (90%+) with moderate accuracy loss.

## Tutorial benchmarks

For runtime measurements on the PolarFire SoC Video Kit hardware, see the [Tutorials — Model Index]({% link tutorials/model-index.md %}).
