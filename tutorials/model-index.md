---
title: Model Index by Source
layout: default
parent: Tutorials
nav_order: 3
---

# Model Index by Source

Tutorials in the SDK are grouped by their upstream source framework or model zoo. The table below lists every tutorial source directory and the kinds of models it contains. For per-model runtime and accuracy, see [Metrics]({% link tutorials_metrics.md %}).

| Source folder | Representative models |
|---|---|
| `darknet/` | yolov2-tiny, yolov2-tiny-voc, yolov3-tiny |
| `kaggle/` | efficientnet-lite0, efficientnet-lite4 |
| `mediapipe/` | efficientnet_lite0, efficientnet_lite2 |
| `onnx/` | resnet18/34/101, squeezenet1.1, scrfd_500m_bnkps, yolov7, yolov9-s, yolov9-m |
| `openvino/` | mobilenet-v1/v2 (several sizes), mobilefacenet-arcface, squeezenet1.0/1.1, deeplabv3 |
| `pytorch/` | torchvision resnet18/50/wide_resnet50_2, inception_v3, googlenet, ssdlite320_mobilenet_v3_large, squeezenet1_0, lpr_eu_v3 |
| `qualcomm/` | DeepLabV3-Plus-MobileNet (quantized variants), Midas-V2, FFNet-78S/122NS (LowRes), MobileNet-v2/v3, GoogLeNet, ResNet18/50/101, WideResNet50, XLSR, QuickSRNet S/M/L, SESR-M5 |
| `tensorflow/` | yolo-v3-tf, yolo-v3-tiny-tf, yolo-v4-tiny-tf, mobilenet_v1/v2 (several sizes), efficientnet-lite0–4 (INT8), posenet |
| `ultralytics/` | yolov5n/s/m (relu and default), yolov8n/s/m (detection, cls, pose, seg, obb), yolov9t/s, yolov3-tinyu, 512×288 variants |
| `PINTO/` | 016_EfficientNet-lite, 046_yolov4-tiny, 081_MiDaS_v2, 132_YOLOX, 307_YOLOv7 |
| `vectorblox/` | yolov8n-relu (Microchip-tuned variant) |
| `compressed/` | yolov8n_comp66, yolov8s_comp68 (structured 2:4 compression) |
| `unstructure_compressed/` | resnet18, yolov5n, yolov8n (detection + pose), yolov9s — with varying unstructured compression rates |

Each tutorial folder contains a single `MODEL_NAME.sh` script plus any configuration (anchor JSON, class-name list) the postprocessing needs. Run one from inside `vbx_env`:

```bash
cd $VBX_SDK/tutorials/<SOURCE>/<MODEL>
bash <MODEL>.sh
```

For the complete list of SDK tutorials with direct links and per-model metrics, see [Metrics]({% link tutorials_metrics.md %}) or browse [`tutorials/` on GitHub](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/master/tutorials).
