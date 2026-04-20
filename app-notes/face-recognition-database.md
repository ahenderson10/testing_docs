---
title: Updating the Face Recognition Database
layout: default
parent: App Notes
nav_order: 2
---

# Updating the Face Recognition Database

This note explains how to build a custom face database from your own images and integrate it into the face recognition demo. The workflow uses Python scripts in the SDK's `faceDemo` example folder.

## Prerequisites

- VectorBlox SDK installed and `vbx_env` activated:

  ```bash
  source $VBX_SDK/setup_vars.sh
  ```

- Tutorial binaries generated for the three required models (see [Preparing models](#preparing-models) below).

## Preparing models

The Python scripts require a face detection model, a face recognition model, and a gender/age model. Run the corresponding SDK tutorials to generate the binaries:

```bash
cd $VBX_SDK/tutorials/onnx/scrfd_500m_bnkps
bash scrfd_500m_bnkps.sh

cd $VBX_SDK/tutorials/mxnet/mobilefacenet-arcface
bash mobilefacenet-arcface.sh

cd $VBX_SDK/tutorials/onnx/genderage
bash genderage.sh
```

Each script downloads the source model, converts it to TFLite INT8, and compiles it to a `.vnnx` binary.

## Creating a new face database

The `faceDemo` folder is at:

```bash
$VBX_SDK/example/python/faceDemo
```

### 1. Prepare your image set

Inside `$VBX_SDK/example/python/faceDemo`, create a `dbImages/` folder. For each person you want to register, create a sub-folder named after them and place one or more images of that person inside it:

```text
dbImages/
├── Alice/
│   ├── alice_photo1.jpg
│   └── alice_photo2.jpg
├── Bob/
│   └── bob_photo1.jpg
```

Using multiple images per person improves matching accuracy because the embeddings from multiple images are combined into a single representative entry.

### 2. Generate the database

```bash
cd $VBX_SDK/example/python/faceDemo
python createDb.py
```

This script processes each image through the detection and recognition models and saves the result to `faceDb.npy` — a NumPy save file containing a Python dictionary of names and embeddings.

{: .note }
`faceDb.npy` is a Python-readable database. It is not the same format as the C-code database embedded in the demo binary. Use `exportDb.py` (see below) to generate the C-code version.

### 3. Test with a static image

Verify the database by running the recognition pipeline on a test image:

```bash
python processImage.py
```

This processes `garden.jpg` (included in the `faceDemo` folder) using `faceDb.npy`. The pipeline:

1. Loads the image from disk.
2. Resizes it if necessary.
3. Runs the SCRFD detection model.
4. Aligns and crops each detected face.
5. Runs ArcFace recognition on each face.
6. Compares embeddings to the database.
7. Draws bounding boxes and names on the image.
8. Saves the annotated output to disk.

### 4. Export the database to C code

```bash
python exportDb.py
```

This generates `faceDb.c`, which contains C code representing the face embeddings. The output format is compatible with the video demo source code.

## Integrating the new database into the demo

Open the face detection demo source file:

```bash
$VBX_SDK/example/soc-video-c/faceDetection/faceDetectDemo.c
```

The default embeddings are defined near the top of the file. Replace them with the contents of your generated `faceDb.c`:

```c
// Replace the existing embedding arrays with those from faceDb.c
// Example entry format:
// float alice_embedding[] = { 0.034f, -0.112f, ... };
```

Rebuild and redeploy the demo on the PolarFire SoC Video Kit:

```bash
cd ~/VectorBlox-SDK-release-v3.0/example/soc-video-c
make clean
make
./run-video-model
```

## Adding faces at runtime (without rebuilding)

You can also add and remove faces while the demo is running without rebuilding the binary. See [Face Recognition Demo — Adding a face at runtime]({% link app-notes/face-recognition-demo.md %}#adding-a-face-to-the-database-at-runtime).

Runtime additions are stored in memory and are lost when the demo exits. For permanent additions, rebuild the binary with the updated `faceDb.c` as described above.
