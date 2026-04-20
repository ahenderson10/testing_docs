---
title: Prerequisites
layout: default
parent: Getting Started
nav_order: 1
---

# Prerequisites

The SDK is developed and tested on Ubuntu. WSL is supported with one caveat — work from your Linux home directory (or a directory with the correct permissions set), not from a mounted Windows path.

## Supported host environments

- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

## Required tools

- `git` and `git-lfs`. The SDK ships large asset files through Git LFS, so the clone will be incomplete without it.
- Up-to-date host drivers. A few Python packages link against system libraries, so stale drivers can cause install-time failures.

## Install `git-lfs`

```bash
sudo apt-get update
sudo apt install git-lfs && git-lfs install
```

---

Source: [VectorBlox-SDK/README.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/README.md)
