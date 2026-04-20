---
title: Supported Operators
layout: default
parent: SDK Overview
nav_order: 3
---

# Supported TFLite INT8 Operators

`vnnx_compile` supports the TFLite INT8 operators listed below. Operators with an entry under **Known Limitations** only work when those parameter values are used (for example, a specific `axis` or `padding` mode, or a constrained set of fused activations). If you need an operator that is not listed, contact the VectorBlox team at `vectorblox@microchip.com`.

| Category | Count |
|---|---|
| Fully supported | 38 |
| Supported with constraints | 36 |
| **Total** | **74** |

## Complete operator list

| Operator | Known limitations |
|---|---|
| ABS | |
| ADD | Fused activation = [NONE, RELU, RELU6] |
| ARG_MAX | Axis = [-1] |
| ARG_MIN | Axis = [-1] |
| AVERAGE_POOL_2D | Fused activation = [NONE, RELU, RELU6]; Padding = [SAME, VALID] |
| CAST | INT8/UINT8 → INT32 only |
| CONCATENATION | Axis = [-4, -3, -2, -1]; Fused activation = [NONE, RELU, RELU6] |
| CONV_2D | Fused activation = [NONE, RELU, RELU6]; Padding = [SAME, VALID] |
| DEPTHWISE_CONV_2D | Fused activation = [NONE, RELU, RELU6]; Padding = [SAME, VALID] |
| DEQUANTIZE | |
| DIV | Fused activation = [NONE, RELU, RELU6]; Input 2 must be constant |
| ELU | |
| EQUAL | |
| EXP | |
| EXPAND_DIMS | Axis = [-4, -3, -2, -1] |
| FULLY_CONNECTED | Fused activation = [NONE, RELU, RELU6] |
| GATHER | Axis = [-4, -3, -2, -1] |
| GELU | |
| GREATER | Axis = [-4, -3, -2, -1] |
| GREATER_EQUAL | |
| HARD_SWISH | |
| LEAKY_RELU | |
| LESS | |
| LESS_EQUAL | |
| LOG | |
| LOGISTIC | |
| MAXIMUM | |
| MAX_POOL_2D | Fused activation = [NONE, RELU, RELU6]; Padding = [SAME, VALID] |
| MEAN | |
| MINIMUM | |
| MUL | Fused activation = [NONE, RELU, RELU6] |
| NEG | |
| NOT_EQUAL | |
| PACK | Axis = [-4, -3, -2, -1] |
| PAD | |
| PADV2 | |
| POW | |
| PRELU | |
| QUANTIZE | |
| REDUCE_MAX | Axis = [-4, -3, -2, -1] |
| REDUCE_MIN | Axis = [-4, -3, -2, -1] |
| REDUCE_PROD | Axis = [-4, -3, -2, -1] |
| RELU | |
| RELU6 | |
| RELU_0_TO_1 | |
| RELU_N1_TO_1 | |
| RESHAPE | |
| RESIZE_BILINEAR | |
| RESIZE_NEAREST_NEIGHBOR | |
| RSQRT | |
| SILU | |
| SLICE | |
| SOFTMAX | Dim = [-3, -2, -1] |
| SPLIT | Axis = [-4, -3, -2, -1] |
| SPLIT_V | Axis = [-4, -3, -2, -1] |
| SQUEEZE | Axis = [-4, -3, -2, -1] |
| STRIDED_SLICE | |
| SUB | Fused activation = [NONE, RELU] |
| SUM | Axis = [-4, -3, -2, -1] |
| TANH | |
| TILE | |
| TRANSPOSE | |
| TRANSPOSE_CONV | Fused activation = [NONE, RELU, RELU6]; Padding = [SAME, VALID] |
| UNPACK | Axis = [-4, -3, -2, -1] |

---

Source: [VectorBlox-SDK/docs/OPS.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/OPS.md)
