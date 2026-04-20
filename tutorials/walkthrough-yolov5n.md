---
title: "Walkthrough: yolov5n"
layout: default
parent: Tutorials
nav_order: 1
---

# Walkthrough: yolov5n_512x288

The SDK includes a set of public models that demonstrate the full VectorBlox flow. This walkthrough follows `tutorials/ultralytics/yolov5n_512x288/yolov5n_512x288.sh` end to end, so you can see what every stage of a tutorial script actually does.

## Run the tutorial

Inside the `vbx_env` virtual environment:

```bash
cd $VBX_SDK/tutorials/ultralytics/yolov5n_512x288
bash yolov5n_512x288.sh
```

<details markdown="block">

<summary>Breakdown of the shell script</summary>

### 1. Generate the calibration NumPy array

Builds a `20 × 288 × 512 × 3` calibration array from the shared COCO sample:

```bash
if [ ! -f $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy ]; then
    generate_npy $VBX_SDK/tutorials/coco2017_rgb_20x416x416x3.npy \
        -o $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy \
        -s 288 512 --norm
fi
```

### 2. Export the YOLOv5n source model

Downloads `coco.names` and the pretrained YOLOv5n PyTorch weights, sets up an isolated Ultralytics virtual environment, and exports the model to ONNX at 288×512:

```bash
[ -f coco.names ] || wget -q https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
if [ ! -f yolov5n_512x288.tflite ]; then
    [ -f yolov5n.pt ] || wget -q https://github.com/ultralytics/yolov5/releases/download/v6.0/yolov5n.pt
    [ -d yolov5 ] || git clone --branch v7.0 https://github.com/ultralytics/yolov5
    if [ -d yolov5 ]; then
        cd yolov5
        python3 -m venv ultralytics
        source ultralytics/bin/activate
        pip install --upgrade pip
        pip install -r requirements.txt
        pip install onnx
        pip install torch==2.5.0 torchvision==0.20.0
        python export.py --weights ../yolov5n.pt --include onnx --imgsz 288 512
        cd ..
        source $VBX_SDK/vbx_env/bin/activate
    fi
fi
```

### 3. Convert, cut, and quantize the ONNX model

Uses `onnx2tf` to produce an INT8 TFLite model. `--output_names_to_interrupt_model_conversion` cuts the graph at the three final convolutions of the detection head so the YOLO decode can run in postprocessing.

```bash
if [ ! -f yolov5n_512x288.tflite ]; then
    echo "Running ONNX2TF..."
    onnx2tf -cind images $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy [[[[0.,0.,0.]]]] [[[[1.,1.,1.]]]] \
        --output_names_to_interrupt_model_conversion \
            "/model.24/m.0/Conv_output_0" \
            "/model.24/m.1/Conv_output_0" \
            "/model.24/m.2/Conv_output_0" \
        -i yolov5n.onnx \
        --output_signaturedefs \
        --output_integer_quantized_tflite
    cp saved_model/yolov5n_full_integer_quant.tflite yolov5n_512x288.tflite
fi
```

### 4. Preprocess the TFLite model

Adds a `uint8 → int8` preprocessing layer so the accelerator accepts raw camera input.

```bash
if [ -f yolov5n_512x288.tflite ]; then
    tflite_preprocess yolov5n_512x288.tflite --scale 255
fi
```

### 5. Compile the VectorBlox binary

Builds a V1000, no-compression `.vnnx`:

```bash
if [ -f yolov5n_512x288.pre.tflite ]; then
    echo "Generating VNNX for V1000 ncomp configuration..."
    vnnx_compile -s V1000 -c ncomp -t yolov5n_512x288.pre.tflite \
        -o yolov5n_512x288_V1000_ncomp.vnnx
fi
```

### 6. Run the binary through the Python simulator

Runs inference on the sample dog image with YOLOv5 postprocessing:

- `-j yolov5n.json` — YOLOv5n anchor configuration.
- `-v 5` — use the v5 postprocessing path.
- `-l coco.names` — class-name labels.
- `-t 0.25` — confidence threshold.

```bash
if [ -f yolov5n_512x288_V1000_ncomp.vnnx ]; then
    echo "Running Simulation..."
    python $VBX_SDK/example/python/yoloInfer.py \
        yolov5n_512x288_V1000_ncomp.vnnx \
        $VBX_SDK/tutorials/test_images/dog.jpg \
        -j yolov5n.json -v 5 -l coco.names -t 0.25

    echo "C Simulation Command:"
    echo '$VBX_SDK/example/sim-c/sim-run-model yolov5n_512x288_V1000_ncomp.vnnx $VBX_SDK/tutorials/test_images/dog.jpg YOLOV5'
fi
```

</details>

## Cross-check with the C simulator

Once the script finishes, verify the binary with the bit-accurate C simulator:

```bash
$VBX_SDK/example/sim-c/sim-run-model yolov5n_512x288_V1000_ncomp.vnnx \
    $VBX_SDK/tutorials/test_images/dog.jpg YOLOV5
```

The C simulator output should line up with the Python result from the bash script.

## Next

[Deploy the binary to a PolarFire SoC Video Kit]({% link tutorials/deploy-to-soc.md %}).

---

Source: [tutorials/ultralytics/yolov5n_512x288](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/tree/master/tutorials/ultralytics/yolov5n_512x288)
