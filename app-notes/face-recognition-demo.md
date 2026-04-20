---
title: Face Recognition Demo
layout: default
parent: App Notes
nav_order: 1
---

# Face Recognition Demo

This application note describes the face recognition demo included with the VectorBlox SDK. The demo is a two-model inference pipeline that detects and identifies faces in a live video stream running on the PolarFire SoC Video Kit.

## System overview

The demo chains two deep learning models in series:

1. **SCRFD** (face detection) — detects faces and returns bounding boxes plus five facial keypoints (eyes, nose, mouth corners).
2. **ArcFace Mobile** (face recognition) — takes a cropped, aligned face image and returns a 128-element embedding vector.

A software tracker links the models and maintains identities over time.

```text
Video input (1920×1080)
    │
    ▼
Resize to 512×288
    │
    ▼
SCRFD face detection (VectorBlox accelerator)
    │  bounding boxes + keypoints
    ▼
Kalman filter tracker
    │  one face per frame (rotates through faces in scene)
    ▼
Affine alignment (accelerator: crop + resize + rotate)
    │
    ▼
ArcFace Mobile recognition (VectorBlox accelerator)
    │  128-element embedding
    ▼
Database lookup (software)
    │  name match (green) or unknown (blue)
    ▼
Annotated video output
```

## Models used

| Model | Task | Input resolution |
|---|---|---|
| [SCRFD 500M BNKPS](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/tutorials/onnx/scrfd_500m_bnkps/scrfd_500m_bnkps.sh) | Face detection | 512×288×3 |
| [ArcFace Mobile](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/tutorials/mxnet/mobilefacenet-arcface/mobilefacenet-arcface.sh) | Face recognition | 112×112×3 |

## How the tracker works

The tracker uses Kalman filters to track detected faces across frames. When multiple faces are present in the scene, the tracker rotates through them — running recognition on one face per frame. This maintains a consistent frame rate while identifying all faces in the field of view.

The recognition model requires the input face to be a specific size and orientation. The tracker converts the five facial keypoints from SCRFD into a transformation matrix. This matrix is passed to the accelerator, which performs the affine crop, resize, and rotation in hardware before passing the aligned face to ArcFace.

## Default face database

The demo ships with four pre-registered entries: **John**, **Nancy**, **Tina**, and **Bob**. These embeddings were generated from a separate video — not the live feed. Recognized faces are outlined in **green** with a name label; unrecognized faces are outlined in **blue**.

A sample face video for testing is available: [SampleFaces.mp4](https://github.com/Microchip-Vectorblox/assets/releases/download/assets/SampleFaces.mp4).

## Running the demo

The face recognition demo runs within the standard VectorBlox video demo on the PolarFire SoC Video Kit. After the demo launches:

1. Use `Enter` to cycle models until **Recognition** mode is selected.
2. The demo automatically detects and attempts to identify faces.

### Adding a face to the database at runtime

In Recognition mode, type `a` and press `Enter` to begin an add operation:

- The largest face on screen is highlighted.
- Type `a` again and press `Enter` to capture that face's embedding.
- You are prompted to enter a name (press `Enter` to use the default ID).

### Removing a face from the database at runtime

Type `d` and press `Enter` in Recognition mode:

- A numbered list of all database entries is displayed.
- Enter the index number of the entry to delete and press `Enter`.
- Press `Enter` without a number to cancel.

## Next steps

To create a completely new face database from your own images (offline), see [Updating the Face Recognition Database]({% link app-notes/face-recognition-database.md %}).
