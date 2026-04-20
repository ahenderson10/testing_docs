---
title: Compression Library
layout: default
nav_order: 5
has_children: true
permalink: /compression/
---

# Compression Library

The VectorBlox Compression Library is a PyTorch training utility that reduces model size and computation while preserving accuracy. It is designed specifically for the VectorBlox accelerator hardware.

## What compression does

The library compresses models in two complementary ways:

**Pruning (sparsity)**
Pruning sets selected weights to zero. Because the hardware knows those weights are zero, it can skip the corresponding multiplications, reducing runtime proportionally. The most common standard is **2:4 semi-structured sparsity** (2 out of every 4 weights are pruned, yielding a 2x compression rate). VectorBlox Compression also supports **1:4 per-layer sparsity**, allowing overall compression rates between 2x and 4x.

**Weight pairing**
Pairing assigns equal magnitudes to pairs of weights. This lets the accelerator replace a multiply-accumulate with a cheaper addition or subtraction before the multiplication unit. Pairing significantly improves accuracy at a given compression rate without changing the number of weights or multiply operations.

**Unstructured compression**
For even higher compression rates, unstructured pruning can be applied without the 2:4 structural constraint, at the cost of less predictable hardware speedup.

## Compression modes

| Mode | VectorBlox job suffix | Target overall sparsity |
|---|---|---|
| No compression | (default) | 0% |
| Structured (2:4 / 1:4) | `COMP` | 50–75% |
| Unstructured | `UCOMP` | 50–90% |

## Performance summary

Structured compression typically reduces multiply operations by **2.5–2.8x** with less than **1.5% accuracy drop** on COCO object detection benchmarks. For full performance tables see [Performance]({% link compression/performance.md %}).

## In this section

| Page | Contents |
|---|---|
| [Installation]({% link compression/install.md %}) | How to install the compression library in your Python environment |
| [Usage]({% link compression/usage.md %}) | PyTorch integration and the `compress_vbx3` API |
| [Performance]({% link compression/performance.md %}) | COCO and ImageNet accuracy and speedup tables |

Source: [VectorBlox-Compression on GitHub](https://github.com/Microchip-Vectorblox/VectorBlox-Compression)
