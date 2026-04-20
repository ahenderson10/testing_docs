---
title: Toolchain & Conversion
layout: default
parent: SDK Overview
nav_order: 1
---

# Toolchain & Conversion

The VectorBlox flow leans on four categories of tools. Depending on where your model starts, you may use only one or two:

- **OpenVINO model zoo and optimizer** (`omz_downloader`, `mo`) to fetch pretrained models and convert them to OpenVINO IR.
- **PINTO's `openvino2tensorflow`** to convert OpenVINO IR to TFLite.
- **PINTO's `onnx2tf`** to convert ONNX to TFLite.
- **VectorBlox SDK helpers** — `generate_npy`, `tflite_quantize`, `tflite_cut`, `tflite_preprocess`, `tflite_postprocess` — to build calibration data, quantize, split graphs, and wire up preprocessing.

All tools are available on the command line after `source setup_vars.sh`.

## OpenVINO model zoo and optimizer

The OpenVINO model zoo provides downloads and metadata for many pretrained networks. Full docs: [OpenVINO Toolkit](https://docs.openvino.ai/2025/index.html).

```bash
omz_downloader --print_all
omz_downloader --name MODEL_NAME
```

`mo` converts source-framework models into OpenVINO IR and applies inference-time optimizations (dropout removal, layer fusion):

```bash
mo --framework caffe --input_model MODEL_NAME.caffe

# With scale and mean values — check the model's documentation to decide.
mo --framework caffe --input_model MODEL_NAME.caffe \
   --scale_values [127.,127.,127.] --mean_values [64.,64.,64.]
```

## PINTO `openvino2tensorflow`

Converts OpenVINO IR to TFLite. Handy for preserving NCHW input layout or tweaking parameters/layers during conversion. Project: [PINTO0309/openvino2tensorflow](https://github.com/PINTO0309/openvino2tensorflow).

```bash
openvino2tensorflow --model-path MODEL.xml --output_full_integer_quant_tflite \
  --load_dest_file_path_for_the_calib_npy CALIB.npy
```

## PINTO `onnx2tf`

Converts ONNX to TFLite, with options to pin static input shapes or cut the graph at a named node. Project: [PINTO0309/onnx2tf](https://github.com/PINTO0309/onnx2tf).

```bash
onnx2tf --input_onnx_file_path MODEL_NAME.onnx --output_integer_quantized_tflite \
  --output_signaturedefs \
  --custom_input_op_name_np_data_path INPUT_OP NUMPY.npy [MEAN] [STD]
```

## `generate_npy`

Builds a NumPy calibration file from a folder of sample images. Specify `--shape` when your model expects something other than 224×224.

```bash
generate_npy sample_images/ --output_name OUTPUT_NAME --shape HEIGHT WIDTH --count 20
```

## `tflite_quantize`

Quantizes a TensorFlow SavedModel to TFLite INT8 using the calibration NumPy data. `--mean` and `--scale` normalize the calibration inputs.

```bash
tflite_quantize saved_model/ MODEL_NAME.tflite --data NUMPY.npy --mean 127.5 --scale 127.5
```

## `tflite_cut`

Reassigns the start/end of a TFLite graph, useful for stripping training-only or unsupported tail ops. Outputs are named `MODEL_NAME.0.tflite`, `MODEL_NAME.1.tflite`, and so on.

```bash
tflite_cut MODEL_NAME.tflite -i 3 -o 5
tflite_cut MODEL_NAME.tflite -c
```

## `tflite_preprocess`

Adds a preprocessing layer on top of a TFLite model so the network can accept `uint8` input directly. This is required when running on hardware or in C, since the runtime gives the model `uint8` data.

```bash
tflite_preprocess MODEL_NAME.tflite -s 255 -m 127
```

## `tflite_postprocess`

Adds an injection layer for the demo that resizes outputs and converts category outputs to pixel values.

```bash
tflite_postprocess MODEL_NAME.tflite --dataset VOC --opacity 0.8 --height 1080 --width 1920
```

---

Source: [VectorBlox-SDK/docs/sdk_toolkit.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/sdk_toolkit.md)
