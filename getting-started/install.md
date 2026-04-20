---
title: Install the SDK
layout: default
parent: Getting Started
nav_order: 2
---

# Install the SDK

## 1. Download the SDK

Either download a release archive from the [VectorBlox-SDK releases page](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/releases), or clone the repository directly:

```bash
git clone https://github.com/Microchip-Vectorblox/VectorBlox-SDK.git
```

On WSL, clone into a directory inside your Linux home so file permissions and Git LFS both behave.

## 2. Install system dependencies

This step requires `sudo` and is only needed once per machine.

```bash
cd VectorBlox-SDK
bash install_dependencies.sh
```

## 3. Activate the Python virtual environment

Running `setup_vars.sh` creates (if missing) and activates a Python 3.10 virtual environment and exports `$VBX_SDK` so the tutorials and example scripts can find their assets. Source it in every new shell:

```bash
source setup_vars.sh
```

Your prompt should now show `(vbx_env)`, and `echo $VBX_SDK` should print the path to the SDK checkout.

---

Source: [VectorBlox-SDK/README.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/README.md)
