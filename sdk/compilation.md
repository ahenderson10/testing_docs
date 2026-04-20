---
title: Graph Compilation
layout: default
parent: SDK Overview
nav_order: 2
---

# Graph Compilation with `vnnx_compile`

`vnnx_compile` turns a preprocessed TFLite INT8 model into a `.vnnx` binary that runs on the CoreVectorBlox hardware and the host simulator. It takes the input TFLite graph, a hardware **size configuration**, and a **compression configuration**. For custom pre- or post-processing, pass `--start_layer` / `--end_layer` to cut the graph.

For full handbook details, refer to the CoreVectorBlox v3.0 IP Handbook.

## Usage

```text
usage: vnnx_compile [-h] -t TFLITE -s {V250,V500,V1000} -c {ncomp,comp,ucomp}
                    [-o OUTPUT] [--start_layer START_LAYER] [-e END_LAYER]
                    [-i [INPUTS ...]] [-m MEAN [MEAN ...]] [-sc SCALE [SCALE ...]]
                    [-b] [-u]

  -t TFLITE            TFLite INT8 model (.tflite)
  -s {V250,V500,V1000} Target hardware size
  -c {ncomp,comp,ucomp} Compression mode
  -o OUTPUT            Output filename
  --start_layer / -e   Start and end nodes (for custom pre/post graphs)
  -i [INPUTS ...]      Embed test input(s) in the model
  -m / -sc             Mean / scale values applied to the test input
  -b                   Interpret input as BGR
  -u                   Treat input as uint8 (only valid with -c ucomp)
```

You can embed a test image so the hardware can self-verify via checksum:

```bash
vnnx_compile -s SIZE_CONFIG -c COMPRESSION_CONFIG -t MODEL_NAME.tflite -i IMAGE_NAME.jpg
```

## Size configurations

| Config | Notes |
|---|---|
| V1000 | Largest fabric footprint, highest throughput. Default for tutorials. |
| V500  | Mid-size. |
| V250  | Smallest fabric footprint. |

See [Resource Utilization]({% link sdk/resource-utilization.md %}) for the LUT/DFF/SRAM/MATH cost of each configuration.

## Compression modes

### No compression (`ncomp`)

Default. The output file is named `MODEL_NAME_<SIZE>_ncomp.vnnx`.

```bash
vnnx_compile -s V1000 -c ncomp -t MODEL_NAME.tflite
```

### Structured compression (`comp`)

Uses the 2:4 / 1:4 sparsity and pairing scheme from the VectorBlox Compression library. Output is `MODEL_NAME_<SIZE>_comp.vnnx`.

```bash
vnnx_compile -s V1000 -c comp -t MODEL_NAME.tflite
```

### Unstructured compression (`ucomp`)

Only V1000 is supported for unstructured compression today. Add `--uint8` when the source graph is still `uint8`; the flag converts inputs to signed `int8` during compile. Output is `MODEL_NAME.ucomp`.

```bash
vnnx_compile -s V1000 -c ucomp -t MODEL_NAME.tflite --uint8
```

`--uint8` is only valid with `-c ucomp`.

---

Source: [VectorBlox-SDK/docs/vbx_graph_generation.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/vbx_graph_generation.md)
