---
title: Running a Tutorial
layout: default
nav_order: 49
---

# Running a Tutorial

The VectorBlox SDK comes with a set of public models that are used to demonstrate how to get a source model to work on the VectorBlox accelerator.

## Running a yolov8n tutorial.

- In the virtual environment, navigate to the [tutorials/ultralytics/yolov5n_512x288] directory.

- Run the script within via the 
 
    ```
    bash yolov5n_512x288.sh
    ```
    
<details markdown="block">

<summary>Breakdown of yolov5n_512x288.sh Shell Script</summary>

## Generation of numpy array for quantization


1. Script below creates a npy array with a shape of [20, 288, 512,3] from source npy array with the shape of [20, 416, 416, 3]


        ```
        if [ ! -f $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy ]; then
            generate_npy $VBX_SDK/tutorials/coco2017_rgb_20x416x416x3.npy -o $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy -s 288 512  --norm 
        fi
        ```


## Export the Yolov5n source model


1. Downloads the coco names and Pytorch model. 


2. Sets up the Ultralytics virtual environment from the Ultralytics repo and the packages


3. Exports the yolov5n pytorch model to an `.onnx` file type with an input size of 288(height) and 512(width)


    ```
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


## Convert, Cut, and Quantize the onnx model


1. Uses onnx2tf to convert an `.onnx` model to `.tflite`


2. Takes the previous `.npy` array and `.onnx` models and generates an int8 `.tflite` model.


3. Cuts the model at various stages via `--output_names_to_interrupt_model_conversion` argument to cut the model at 3 areas in the model


    ```
        if [ ! -f yolov5n_512x288.tflite ]; then
        echo "Running ONNX2TF..."
        onnx2tf -cind images $VBX_SDK/tutorials/coco2017_rgb_norm_20x288x512x3.npy [[[[0.,0.,0.]]]] [[[[1.,1.,1.]]]] \
        --output_names_to_interrupt_model_conversion "/model.24/m.0/Conv_output_0" "/model.24/m.1/Conv_output_0" "/model.24/m.2/Conv_output_0" \
        -i yolov5n.onnx \
        --output_signaturedefs \
        --output_integer_quantized_tflite
        cp saved_model/yolov5n_full_integer_quant.tflite yolov5n_512x288.tflite
        fi    
    ```


## Preprocess the tflite model


1. A preprocessing layer is added to the model so that the input can be fed in as uint8 rather than int8.


    ```
        if [ -f yolov5n_512x288.tflite ]; then
            tflite_preprocess yolov5n_512x288.tflite  --scale 255
        fi    
    ```


## Compile the Vectorblox Binary


1. Generates a V1000 Vectorblox Binary with the no compression(ncomp) flag.


    ```
        if [ -f yolov5n_512x288.pre.tflite ]; then
            echo "Generating VNNX for V1000 ncomp configuration..."
            vnnx_compile -s V1000 -c ncomp -t yolov5n_512x288.pre.tflite  -o yolov5n_512x288_V1000_ncomp.vnnx
        fi    
    ```


## Run the VectorBlox Binary through Python simulation


1. Runs inference on the generated vnnx binary with the following args:


    - `-j yolov5n.json`:   yolov5n postprocessing anchors


    - `-v 5`:              use yolov5n postprocessing script


    - `-l coco.names`:     names to append to classes for detection


    - `-t 0.25`:           minimum threshold for detection (from 0.0-1.0)



2. In addition, it shows the instruction to run to obtain a similar result from the C simulation


    ```
        if [ -f yolov5n_512x288_V1000_ncomp.vnnx ]; then
            echo "Running Simulation..."
            python $VBX_SDK/example/python/yoloInfer.py yolov5n_512x288_V1000_ncomp.vnnx $VBX_SDK/tutorials/test_images/dog.jpg -j yolov5n.json -v 5 -l coco.names -t 0.25 
            echo "C Simulation Command:"
            echo '$VBX_SDK/example/sim-c/sim-run-model yolov5n_512x288_V1000_ncomp.vnnx $VBX_SDK/tutorials/test_images/dog.jpg YOLOV5'
        fi
    ```


    </details> 
    


- Run the VectorBlox Binary through C simulation

    ```
    $VBX_SDK/example/sim-c/sim-run-model yolov5n_512x288_V1000_ncomp.vnnx $VBX_SDK/tutorials/test_images/dog.jpg YOLOV5
    ```

- Check if the results obtained from the C simulation is close to Python results from bash script.


[Deployment of Tutorial Binary to PolarFire SoC Video Kit]
----
[Deployment of Tutorial Binary to PolarFire SoC Video Kit]: tutorial_soc
[tutorials/ultralytics/yolov5n_512x288]: https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/tutorials/ultralytics/yolov8n/