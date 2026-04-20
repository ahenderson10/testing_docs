# Introduction
This repository holds a Python library to compress models for Pytorch training. Compression includes pruning (sparsity) and weight pairing.
This library is intended for use with the VectorBlox accelerator and is maintained by Microchip Technology.
See https://github.com/Microchip-Vectorblox/VectorBlox-SDK for more details about VectorBlox.

# Features
This library compresses in two ways: pruning and pairing. Pruning assigns weights the value zero, which can allow the accelerator to skip that multiplication since the result will be zero. Pairing assigns a pair of weights the same magnitude, allowing the accelerator to add or subtract the two activations before multiplication. This turns a multiply and accumulate into a cheaper addition or subtraction. This is especially efficient on hardware that has a pre-adder before the multiplication unit.

![Alt text for the image](assets/compression.png)

The most common standard for pruning/sparsity is 2:4 semi-structured sparsity. In this scheme, 2 of every 4 weights are pruned for a 2x compression rate. While Vectorblox Compression supports 2:4, it also supports 1:4 per-layer, allowing for an overall compression rate between 2x and 4x. Using pairing yields a significantly accuracy boost without changing the number of weights or multiplies. Together, these two techniques can create a model that is both more compressed and more accurate than the standard 2:4 scheme. Unstructured compression also works with pairing, allowing even higher compression rates.

# Installation
To use this library, copy compress.py and import into your project.
This requires torch and numpy.
This library can operate on CPU or GPU(s).

# Usage
VectorBlox Compression can be inserted into a training codebase in as few as two lines of code.
```python
import compress
compress.compress_vbx3(model, compression=compression_rate)
```

Our recommended workflow is as follows. First, train the model as usual with no compression. Second, call the compression library and run the training again. This second training (fine-tuning) can be run with the same hyperparameters, though it will often work with fewer epochs.

The `compress_vbx3` method takes a PyTorch model that supports the `named_modules` method. A hook is inserted into compressed layers, allowing further training with minimal code changes. The `compression_rate` parameter sets the target for the overall model compression, and should be between 0.5 and 0.75. For more control, the `json_file` parameter can be used to specify the compression ratio for each layer.

The `compress_vbx3_unstructured` method takes a PyTorch model and performs unstructured compression. the `compression` rate should be between 0.5 and 1.0.

# Models
## COCO Object Detection
![Alt text for the image](assets/detection.png)
### VectorBlox Compression
Model|Accuracy|MMult|MParams|$\Delta$ Acc|Mult Ratio
-----|-------:|----:|------:|---------:|---------:
yolov8n|37.36|4386|3.160|-|-
yolov8n_comp66|36.26|1659|1.095|-1.10|2.64
yolov8s|44.97|14328|11.165|-|-
yolov8s_comp68|43.75|5153|3.629|-1.22|2.78
yolov8m|50.33|39516|25.895|-|-
yolov8m_comp69|49.30|14264|8.090|-1.03|2.77
yolov9t|38.35|4134|2.102|-|-
yolov9t_comp61|37.20|1669|0.830|-1.15|2.48
yolov9s|46.54|13485|7.207|-|-
yolov9s_comp68|45.72|5014|2.295|-0.82|2.69
yolov9m|51.53|38477|20.079|-|-
yolov9m_comp67|50.49|14112|6.546|-1.04|2.73
yolo11n|39.38|3288|2.650|-|-
yolo11n_comp63|38.05|1374|0.981|-1.33|2.39
yolo11s|46.92|10826|9.452|-|-
yolo11s_comp68|45.51|4052|3.028|-1.41|2.67
yolo11m|51.53|34120|20.100|-|-
yolo11m_comp65|50.61|13310|7.048|-0.92|2.56
### VectorBlox Unstructured Compression
Model|Accuracy|MMult|MParams|$\Delta$ Acc|Mult Ratio
-----|-------:|----:|------:|---------:|---------:
yolov8n|37.36|4386|3.160|-|-
yolov8n_ucomp70|36.57|1514|0.958|-0.79|2.90
yolov8n_ucomp75|36.18|1316|0.801|-1.18|3.33
yolov8n_ucomp80|35.93|1112|0.644|-1.43|3.95
yolov8n_ucomp85|34.76|898|0.486|-2.60|4.88
yolov8n_ucomp90|32.97|665|0.330|-4.39|6.59
yolov9t|38.35|4134|2.102|-|-
yolov9t_ucomp80|35.66|997|0.434|-2.69|4.15
yolov9s|46.54|13485|7.207|-|-
yolov9s_ucomp80|45.51|3377|1.460|-1.03|3.99
yolov9s_ucomp85|44.22|2677|1.101|-2.32|5.04
yolo11n|39.38|3288|2.650|-|-
yolo11n_ucomp75|38.21|1131|0.797|-1.17|2.91
yolo11n_ucomp80|37.96|1061|0.749|-1.42|3.10
## Imagenet Classification
### VectorBlox Compression
Model|Accuracy|MMult|MParams|$\Delta$ Acc|Mult Ratio
-----|-------:|----:|------:|---------:|---------:
resnet18|70.42|1814|11.685|-|-
resnet18_comp66|70.12|880|4.336|-0.30|2.06
resnet18_comp70|69.83|836|3.894|-0.59|2.17
mobilenet_v3_large|75.27|217|5.471|-|-
mobilenet_v3_large_comp56|72.89|107|3.883|-2.38|2.03
### VectorBlox Unstructured Compression
Model|Accuracy|MMult|MParams|$\Delta$ Acc|Mult Ratio
-----|-------:|----:|------:|---------:|---------:
resnet18|70.42|1814|11.685|-|-
resnet18_ucomp75|70.24|636|3.317|-0.17|2.85
resnet18_ucomp85|69.56|490|2.201|-0.85|3.70
resnet18_ucomp90|68.50|407|1.643|-1.91|4.45
resnet18_ucomp93|66.98|352|1.308|-3.43|5.16
mobilenet_v3_large|75.27|217|5.471|-|-
mobilenet_v3_large_ucomp80|73.34|85|3.190|-1.93|2.57