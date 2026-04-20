---
title: Usage
layout: default
parent: Compression Library
nav_order: 2
---

# Using the Compression Library

The VectorBlox Compression Library can be integrated into an existing PyTorch training pipeline in as few as two lines of code.

## Minimal example

```python
import compress

# Apply structured compression (target 66% sparsity overall)
compress.compress_vbx3(model, compression=0.66)
```

After calling `compress_vbx3`, continue your training loop as normal. The library inserts hooks into the compressed layers transparently.

## Recommended workflow

1. **Train as usual** — run your full training loop without compression until the model converges.
2. **Apply compression** — call `compress_vbx3` (or `compress_vbx3_unstructured`) with your chosen compression rate.
3. **Fine-tune** — run the training loop again (fine-tuning) with the same hyperparameters. Fewer epochs are often sufficient, typically 25–50% of the original training length.

```python
import torch
import compress

# -- Step 1: load your pre-trained model
model = MyModel()
model.load_state_dict(torch.load("pretrained_weights.pth"))

# -- Step 2: apply structured compression at ~66% sparsity
compress.compress_vbx3(model, compression=0.66)

# -- Step 3: fine-tune
optimizer = torch.optim.SGD(model.parameters(), lr=1e-4, momentum=0.9)
for epoch in range(num_finetune_epochs):
    train_one_epoch(model, optimizer, train_loader)
```

## API reference

### `compress.compress_vbx3(model, compression, json_file=None)`

Applies structured (2:4 / 1:4) pruning with weight pairing to a PyTorch model.

| Parameter | Type | Description |
|---|---|---|
| `model` | `torch.nn.Module` | Any PyTorch model that supports `named_modules` |
| `compression` | `float` | Target overall compression rate. Must be between **0.5** and **0.75** |
| `json_file` | `str` (optional) | Path to a JSON file specifying per-layer compression ratios for fine-grained control |

### `compress.compress_vbx3_unstructured(model, compression)`

Applies unstructured pruning with weight pairing.

| Parameter | Type | Description |
|---|---|---|
| `model` | `torch.nn.Module` | Any PyTorch model that supports `named_modules` |
| `compression` | `float` | Target overall compression rate. Must be between **0.5** and **1.0** |

{: .note }
Unstructured compression achieves higher compression rates (up to 90%) but offers less predictable hardware speedup compared to structured compression. It corresponds to the `UCOMP` job file variant on the PolarFire SoC Video Kit.

## Per-layer compression with a JSON file

For fine-grained control, pass a JSON file specifying the compression ratio for each layer:

```json
{
  "layer1.conv1": 0.5,
  "layer1.conv2": 0.66,
  "layer2.conv1": 0.75
}
```

```python
compress.compress_vbx3(model, compression=0.66, json_file="layer_config.json")
```

Layers not listed in the JSON file use the global `compression` value.

## Exporting and using the compressed model

After fine-tuning, export your model to ONNX or TFLite as you normally would. The compression information is baked into the trained weights — no special export step is required.

Then compile the model with the matching VectorBlox SDK compile flag:

```bash
# Structured compression
vnnx_compile -c V1000 --vnnx-version 3 --format nchw --compression comp model.tflite

# Unstructured compression
vnnx_compile -c V1000 --vnnx-version 3 --format nchw --compression ucomp model.tflite
```

Use the matching `COMP` or `UCOMP` job file on the hardware.

## Next steps

See [Performance]({% link compression/performance.md %}) for accuracy and speedup benchmarks on COCO object detection and ImageNet classification.
