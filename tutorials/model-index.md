---
title: Model Index by Source
layout: default
parent: Tutorials
nav_order: 3
---

# Model Index by Source

Tutorials in the SDK are grouped by their upstream source framework or model zoo. The table below lists every tutorial source directory and the kinds of models it contains. For per-model runtime and accuracy, see the [Tutorial Metrics](#tutorial-metrics) section below.

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

For a browsable list of every tutorial script, see [`tutorials/` on GitHub](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/master/tutorials).

---

## Tutorial metrics {#tutorial-metrics}

Runtime in milliseconds (ms) measured on the [PolarFire SoC Video Kit]({% link hardware/polarfire-soc-video-kit.md %}). Accuracy measured over 1,000 samples. TFLITE and VNNX columns show metric values (Top-1 accuracy or mAP50–95 as indicated).

<details markdown="block">
<summary>No Compression — runtime and accuracy</summary>

| Source | Tutorial | Input (H,W,C) | Runtime (ms) | Task | Metric | TFLITE | VNNX |
|---|---|---|---|---|---|---|---|
| darknet | yolov2-tiny-voc | [416, 416, 3] | 44.0 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| darknet | yolov2-tiny | [416, 416, 3] | 33.3 | object detection | mAP⁵⁰⁻⁹⁵ | 11.1 | 11.1 |
| darknet | yolov3-tiny | [416, 416, 3] | 35.1 | object detection | mAP⁵⁰⁻⁹⁵ | 10.9 | 10.8 |
| kaggle | efficientnet-lite0 | [224, 224, 3] | 19.1 | classification | Top1 | 70.5 | 70.4 |
| kaggle | efficientnet-lite4 | [224, 224, 3] | 89.3 | classification | Top1 | 77.8 | |
| mediapipe | efficientnet_lite0 | [224, 224, 3] | 19.1 | classification | Top1 | 70.5 | 70.4 |
| mediapipe | efficientnet_lite2 | [260, 260, 3] | 38.7 | classification | Top1 | 70.7 | 71.3 |
| onnx | onnx_resnet18-v1 | [224, 224, 3] | 26.9 | classification | Top1 | 69.3 | 69.2 |
| onnx | onnx_resnet34-v1 | [224, 224, 3] | 49.3 | classification | Top1 | 72.6 | 72.4 |
| onnx | onnx_resnet101-v1 | [224, 224, 3] | 163.0 | classification | Top1 | 72.0 | 70.0 |
| onnx | onnx_squeezenet1.1 | [224, 224, 3] | 6.7 | classification | Top1 | 54.0 | 54.5 |
| onnx | scrfd_500m_bnkps | [288, 512, 3] | 11.5 | face detection | | | |
| onnx | yolov7 | [640, 640, 3] | 1073.4 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| onnx | yolov9-s | [640, 640, 3] | 211.3 | object detection | mAP⁵⁰⁻⁹⁵ | 36.0 | 46.8 |
| onnx | yolov9-m | [640, 640, 3] | 729.5 | object detection | mAP⁵⁰⁻⁹⁵ | 59.2 | 59.4 |
| openvino | mobilenet-v1-1.0-224 | [224, 224, 3] | 13.4 | classification | Top1 | 70.1 | 70.1 |
| openvino | mobilenet-v2 | [224, 224, 3] | 18.1 | classification | Top1 | 72.5 | 72.5 |
| openvino | mobilenet-v1-1.0-224-tf | [224, 224, 3] | 13.2 | classification | Top1 | 69.7 | 69.6 |
| openvino | mobilenet-v2-1.0-224 | [224, 224, 3] | 15.1 | classification | Top1 | 70.5 | 70.5 |
| openvino | mobilenet-v2-1.4-224 | [224, 224, 3] | 23.0 | classification | Top1 | 75.3 | 75.2 |
| openvino | mobilefacenet-arcface | [224, 224, 3] | 18.8 | face comparison | | | |
| openvino | squeezenet1.0 | [227, 227, 3] | 15.6 | classification | Top1 | 56.9 | 56.8 |
| openvino | squeezenet1.1 | [227, 227, 3] | 7.3 | classification | Top1 | 57.0 | 56.7 |
| openvino | mobilenet-v1-0.25-128 | [128, 128, 3] | 1.6 | classification | Top1 | 20.8 | 20.9 |
| openvino | deeplabv3 | [513, 513, 3] | 270.8 | segmentation | | | |
| PINTO | 016_EfficientNet-lite | [224, 224, 3] | 19.1 | classification | Top1 | 70.5 | 70.4 |
| PINTO | 046_yolov4-tiny | [416, 416, 3] | 47.4 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| PINTO | 081_MiDaS_v2 | [256, 256, 3] | 127.4 | depth estimation | | | |
| PINTO | 132_YOLOX | [416, 416, 3] | 19.9 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| PINTO | 307_YOLOv7 | [640, 640, 3] | 61.9 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| pytorch | torchvision_resnet18 | [224, 224, 3] | 27.0 | classification | Top1 | 69.0 | 68.9 |
| pytorch | torchvision_resnet50 | [224, 224, 3] | 97.0 | classification | Top1 | 80.8 | 80.7 |
| pytorch | torchvision_wide_resnet50_2 | [224, 224, 3] | 226.5 | classification | Top1 | 81.3 | 80.8 |
| pytorch | torchvision_inception_v3 | [299, 299, 3] | 145.3 | classification | Top1 | 78.1 | 76.1 |
| pytorch | torchvision_ssdlite320_mobilenet_v3_large | [320, 320, 3] | 41.7 | object detection | mAP⁵⁰⁻⁹⁵ | | |
| pytorch | torchvision_googlenet | [224, 224, 3] | 31.0 | classification | Top1 | 62.5 | 62.3 |
| pytorch | lpr_eu_v3 | [34, 146, 3] | 4.5 | plate recognition | | | |
| pytorch | torchvision_squeezenet1_0 | [227, 227, 3] | 15.4 | classification | Top1 | 59.5 | 59.3 |
| qualcomm | DeepLabV3-Plus-MobileNet-Quantized | [520, 520, 3] | 651.1 | segmentation | | | |
| qualcomm | DeepLabV3-Plus-MobileNet_512x288 | [288, 512, 3] | 311.1 | segmentation | | | |
| qualcomm | Midas-V2-Quantized | [256, 256, 3] | 127.1 | depth estimation | | | |
| qualcomm | Midas-V2_256x128 | [128, 256, 3] | 74.9 | depth estimation | | | |
| qualcomm | FFNet-122NS-LowRes | [512, 1024, 3] | 201.2 | segmentation | | | |
| qualcomm | FFNet-122NS-LowRes_512x288 | [288, 512, 3] | 81.4 | segmentation | | | |
| qualcomm | FFNet-78S-LowRes | [512, 1024, 3] | 249.2 | segmentation | | | |
| qualcomm | FFNet-78S-LowRes_512x288 | [288, 512, 3] | 95.0 | segmentation | | | |
| qualcomm | MobileNet-v2-Quantized | [224, 224, 3] | 15.0 | classification | Top1 | 67.1 | 66.5 |
| qualcomm | GoogLeNetQuantized | [224, 224, 3] | 30.9 | classification | Top1 | 67.8 | 68.5 |
| qualcomm | ResNet18Quantized | [224, 224, 3] | 28.5 | classification | Top1 | 66.3 | 68.8 |
| qualcomm | ResNet50Quantized | [224, 224, 3] | 98.3 | classification | Top1 | 74.8 | 76.3 |
| qualcomm | ResNet101Quantized | [224, 224, 3] | 166.8 | classification | Top1 | 73.6 | 75.8 |
| qualcomm | WideResNet50-Quantized | [224, 224, 3] | 226.5 | classification | Top1 | 76.4 | 77.1 |
| qualcomm | MobileNet-v3-Large-Quantized | [224, 224, 3] | 22.4 | classification | Top1 | 69.6 | 68.8 |
| qualcomm | XLSR-Quantized | [128, 128, 3] | 8.7 | image enhancement | | | |
| qualcomm | QuickSRNetSmall-Quantized | [128, 128, 3] | 6.2 | image enhancement | | | |
| qualcomm | QuickSRNetMedium-Quantized | [128, 128, 3] | 9.9 | image enhancement | | | |
| qualcomm | QuickSRNetLarge-Quantized | [128, 128, 3] | 66.5 | image enhancement | | | |
| qualcomm | SESR-M5-Quantized | [128, 128, 3] | 69.4 | image enhancement | | | |
| tensorflow | yolo-v4-tiny-tf | [416, 416, 3] | 47.6 | object detection | mAP⁵⁰⁻⁹⁵ | 12.3 | 12.5 |
| tensorflow | mobilenet_v2 | [224, 224, 3] | 15.0 | classification | Top1 | 70.2 | 70.4 |
| tensorflow | yolo-v3-tiny-tf | [416, 416, 3] | 35.1 | object detection | mAP⁵⁰⁻⁹⁵ | 10.9 | 11.0 |
| tensorflow | yolo-v3-tf | [416, 416, 3] | 592.7 | object detection | mAP⁵⁰⁻⁹⁵ | 34.7 | 34.5 |
| tensorflow | efficientnet-lite0-int8 | [224, 224, 3] | 19.1 | classification | Top1 | 70.5 | 70.4 |
| tensorflow | efficientnet-lite1-int8 | [240, 240, 3] | 26.6 | classification | Top1 | 72.3 | 71.1 |
| tensorflow | efficientnet-lite2-int8 | [260, 260, 3] | 38.7 | classification | Top1 | 71.1 | 71.3 |
| tensorflow | efficientnet-lite3-int8 | [280, 280, 3] | 54.0 | classification | Top1 | 76.6 | 76.1 |
| tensorflow | efficientnet-lite4-int8 | [300, 300, 3] | 89.0 | classification | Top1 | 77.8 | 77.8 |
| tensorflow | mobilenet_v1_050_160 | [160, 160, 3] | 3.8 | classification | Top1 | 49.4 | 49.9 |
| tensorflow | mobilenet_v2_140_224 | [224, 224, 3] | 22.9 | classification | Top1 | 75.5 | 75.4 |
| tensorflow | posenet | [273, 481, 3] | 44.8 | pose detection | | | |
| ultralytics | yolov5n.relu | [416, 416, 3] | 20.3 | object detection | mAP⁵⁰⁻⁹⁵ | 19.2 | 19.1 |
| ultralytics | yolov5s.relu | [416, 416, 3] | 69.7 | object detection | mAP⁵⁰⁻⁹⁵ | 31.5 | 31.6 |
| ultralytics | yolov5n | [640, 640, 3] | 49.4 | object detection | mAP⁵⁰⁻⁹⁵ | 22.9 | 22.9 |
| ultralytics | yolov5n_512x288 | [288, 512, 3] | 18.5 | object detection | mAP⁵⁰⁻⁹⁵ | 20.9 | 20.9 |
| ultralytics | yolov5s | [640, 640, 3] | 172.8 | object detection | mAP⁵⁰⁻⁹⁵ | 33.2 | 33.0 |
| ultralytics | yolov5m | [640, 640, 3] | 540.3 | object detection | mAP⁵⁰⁻⁹⁵ | 41.3 | 41.5 |
| ultralytics | yolov8n_FULL | [640, 640, 3] | 120.0 | object detection | mAP⁵⁰⁻⁹⁵ | 35.4 | 35.4 |
| ultralytics | yolov8s_FULL | [640, 640, 3] | 284.1 | object detection | mAP⁵⁰⁻⁹⁵ | 43.7 | 43.6 |
| ultralytics | yolov8m_FULL | [640, 640, 3] | 752.1 | object detection | mAP⁵⁰⁻⁹⁵ | 47.2 | 47.2 |
| ultralytics | yolov9t_FULL | [640, 640, 3] | 134.1 | object detection | mAP⁵⁰⁻⁹⁵ | 35.5 | 35.6 |
| ultralytics | yolov9s_FULL | [640, 640, 3] | 247.8 | object detection | mAP⁵⁰⁻⁹⁵ | 44.3 | 44.3 |
| ultralytics | yolov8n | [640, 640, 3] | 63.2 | object detection | mAP⁵⁰⁻⁹⁵ | 37.4 | 37.4 |
| ultralytics | yolov8n_argmax | [640, 640, 3] | 64.1 | object detection | mAP⁵⁰⁻⁹⁵ | 37.4 | 37.4 |
| ultralytics | yolov8n_512x288 | [288, 512, 3] | 23.6 | object detection | mAP⁵⁰⁻⁹⁵ | 1.6 | 1.5 |
| ultralytics | yolov8n_512x288_argmax | [288, 512, 3] | 24.1 | object detection | mAP⁵⁰⁻⁹⁵ | 1.6 | 1.5 |
| ultralytics | yolov8n-pose_512x288 | [288, 512, 3] | 25.0 | pose detection | | | |
| ultralytics | yolov8n-pose_512x288_split | [288, 512, 3] | 24.8 | pose detection | | | |
| ultralytics | yolov8s | [640, 640, 3] | 227.6 | object detection | mAP⁵⁰⁻⁹⁵ | 47.8 | 46.9 |
| ultralytics | yolov8m | [640, 640, 3] | 695.8 | object detection | mAP⁵⁰⁻⁹⁵ | 52.1 | 52.1 |
| ultralytics | yolov9s | [640, 640, 3] | 190.9 | object detection | mAP⁵⁰⁻⁹⁵ | 47.8 | 47.9 |
| ultralytics | yolov9t | [640, 640, 3] | 77.0 | object detection | mAP⁵⁰⁻⁹⁵ | 37.9 | 38.1 |
| ultralytics | yolov8n-cls | [224, 224, 3] | 5.2 | classification | Top1 | 67.2 | 67.3 |
| ultralytics | yolov8s-cls | [224, 224, 3] | 12.9 | classification | Top1 | 72.6 | 72.5 |
| ultralytics | yolov8m-cls | [224, 224, 3] | 36.0 | classification | Top1 | 75.5 | 75.9 |
| ultralytics | yolov8n-pose | [640, 640, 3] | 66.7 | pose detection | | | |
| ultralytics | yolov8n-seg | [640, 640, 3] | 80.7 | instance segmentation | | | |
| ultralytics | yolov8n-obb | [1024, 1024, 3] | 164.7 | obb detection | mAP50–95 | | |
| ultralytics | yolov3-tinyu_FULL | [640, 640, 3] | 136.9 | object detection | mAP⁵⁰⁻⁹⁵ | 29.6 | 29.5 |
| ultralytics | yolov5nu_FULL | [640, 640, 3] | 119.6 | object detection | mAP⁵⁰⁻⁹⁵ | 33.2 | 32.7 |
| ultralytics | yolov5m.relu | [416, 416, 3] | 175.9 | object detection | mAP⁵⁰⁻⁹⁵ | 38.2 | 38.3 |
| ultralytics | yolov8x-cls | [224, 224, 3] | 276.2 | classification | | | |
| ultralytics | yolov8l-cls | [224, 224, 3] | 130.4 | classification | | | |
| vectorblox | yolov8n-relu | [640, 640, 3] | 119.5 | object detection | mAP⁵⁰⁻⁹⁵ | 33.3 | 33.2 |

</details>

<details markdown="block">
<summary>Structured compression (COMP) — runtime and accuracy</summary>

| Source | Tutorial | Input (H,W,C) | Runtime (ms) | Task | Metric | TFLITE | VNNX |
|---|---|---|---|---|---|---|---|
| compressed | yolov8n_comp66 | [640, 640, 3] | 45.9 | object detection | mAP⁵⁰⁻⁹⁵ | 37.5 | 37.6 |
| compressed | yolov8s_comp68 | [640, 640, 3] | 108.0 | object detection | mAP⁵⁰⁻⁹⁵ | 46.2 | 46.1 |

</details>

<details markdown="block">
<summary>Unstructured compression (UCOMP) — runtime and accuracy</summary>

| Source | Tutorial | Input (H,W,C) | Runtime (ms) | Task | Metric | TFLITE |
|---|---|---|---|---|---|---|
| unstructure_compressed | resnet18_86s_07p | [224, 224, 3] | 14.3 | classification | Top1 | 65.6 |
| unstructure_compressed | yolov5n_70s_512x512 | [512, 512, 3] | 19.8 | object detection | mAP⁵⁰⁻⁹⁵ | 20.2 |
| unstructure_compressed | yolov8n_50s_25p_512x288 | [288, 512, 3] | 15.4 | object detection | mAP⁵⁰⁻⁹⁵ | 34.6 |
| unstructure_compressed | yolov8n_50s_25p_512x512 | [512, 512, 3] | 25.3 | object detection | mAP⁵⁰⁻⁹⁵ | |
| unstructure_compressed | yolov8n_pose_50s_25p_512x288_split | [288, 512, 3] | 15.5 | pose detection | | |
| unstructure_compressed | yolov9s_70s_15p_512x288 | [288, 512, 3] | 33.0 | object detection | | 43.2 |
| unstructure_compressed | yolov9s_70s_15p_512x512 | [512, 512, 3] | 55.6 | object detection | | |

</details>
