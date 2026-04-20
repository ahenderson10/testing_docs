---
title: Installation
layout: default
parent: Compression Library
nav_order: 1
---

# Installing the VectorBlox Compression Library

The compression library is a single Python file that you copy into your project. There is no separate pip package.

## Requirements

| Dependency | Version |
|---|---|
| Python | 3.8+ (Python 3.10 recommended to match the SDK virtual environment) |
| PyTorch | Any version that supports `named_modules` on your model |
| NumPy | Any recent version |

The library runs on CPU or GPU. No FPGA hardware is required to train or compress a model.

## Installation steps

1. Clone or download the [VectorBlox-Compression](https://github.com/Microchip-Vectorblox/VectorBlox-Compression) repository:

   ```bash
   git clone https://github.com/Microchip-Vectorblox/VectorBlox-Compression.git
   ```

2. Copy `compress.py` from the repository root into your training project directory:

   ```bash
   cp VectorBlox-Compression/compress.py /path/to/your/project/
   ```

3. Import it in your training script:

   ```python
   import compress
   ```

## Using within the VectorBlox SDK virtual environment

If you are already working inside the VectorBlox SDK Python virtual environment (`vbx_env`), PyTorch and NumPy are already installed. Copy `compress.py` into your project and import it directly.

To activate the SDK virtual environment:

```bash
source $VBX_SDK/setup_vars.sh
```

## Next steps

See [Usage]({% link compression/usage.md %}) for how to integrate `compress_vbx3` into your training loop.
