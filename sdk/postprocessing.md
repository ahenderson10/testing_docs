---
title: Postprocessing Functions
layout: default
parent: SDK Overview
nav_order: 6
---

# C Postprocessing Functions

The SDK provides a set of C postprocessing routines that run on the soft processor after the accelerator has produced raw INT8 outputs. Select one by passing its name as the `POSTPROCESSTYPE` argument to `sim-run-model` or `run-model` (for example, `CLASSIFY`, `YOLOV5`, `ULTRALYTICS_CUT`, `SCRFD`, `ARCFACE`, `POSENET`).

## `decodeMultiplePoses_int8`

Postprocessing for posenet-style networks. Returns the number of detected poses along with per-pose keypoints, scores, and displacement information.

```c
int decodeMultiplePoses_int8(poses_t poses[],
                             int8_t *scores,
                             int8_t *offsets,
                             int8_t *displacementsFwd,
                             int8_t *displacementsBwd,
                             int    outputStride,
                             int    maxPoseDetections,
                             fix16_t scoreThreshold,
                             int    nmsRadius,
                             fix16_t minPoseScore,
                             int    height,
                             int    width,
                             int    zero_points[],
                             fix16_t scale_outs[]);
```

Key parameters: `scoreThreshold` and `minPoseScore` are 0.0–1.0 confidence cutoffs; `nmsRadius` is in pixels; `zero_points` / `scale_outs` hold the per-output INT8 zero points and fix16 scales.

## `post_process_classifier_int8`

Classifier topk. Returns the indices of the `topk` highest outputs, sorted highest to lowest.

```c
void post_process_classifier_int8(int8_t *outputs,
                                  const int output_size,
                                  int16_t *output_index,
                                  int topk);
```

## `post_process_scrfd_int8`

Face detection postprocessing for the SCRFD model. Stores bounding box and keypoints into each `object_t` face.

```c
int post_process_scrfd_int8(object_t faces[],
                            int max_faces,
                            int8_t *network_outputs[9],
                            int zero_points[],
                            fix16_t scale_outs[],
                            int image_width,
                            int image_height,
                            fix16_t confidence_threshold,
                            fix16_t nms_threshold,
                            model_t *model);
```

## `post_process_ssd_torch_int8`

Postprocessing for SSD V2 detectors.

```c
int post_process_ssd_torch_int8(fix16_box *boxes,
                                int max_boxes,
                                int8_t *network_outputs[12],
                                fix16_t network_scales[12],
                                int32_t network_zeros[12],
                                int num_classes,
                                fix16_t confidence_threshold,
                                fix16_t nms_threshold);
```

## `post_process_ultra_int8`

Ultralytics-family detection postprocessing (YOLOv8 detection, pose, OBB).

```c
int post_process_ultra_int8(int8_t **outputs,
                            int *outputs_shape[],
                            fix16_t *post,
                            fix16_t thresh,
                            int zero_points[],
                            fix16_t scale_outs[],
                            const int max_boxes,
                            const int is_obb,
                            const int is_pose);
```

Set `is_obb` for oriented-bounding-box models and `is_pose` for pose detection.

## `post_process_ultra_nms` / `post_process_ultra_nms_int8`

Non-maximal suppression for Ultralytics detection output. `post_process_ultra_nms_int8` is the INT8 fast path; the `fix16_t` variant runs on already-scaled fix16 values.

```c
int post_process_ultra_nms(fix16_t *output, int output_boxes,
                           int input_h, int input_w,
                           fix16_t thresh, fix16_t overlap,
                           fix16_box fix16_boxes[], poses_t poses[],
                           int boxes_len, const int num_classes,
                           const int is_obb, const int is_pose);

int post_process_ultra_nms_int8(int8_t *output, int output_boxes,
                                int input_h, int input_w,
                                fix16_t f16_scale, int32_t zero_point,
                                fix16_t thresh, fix16_t overlap,
                                fix16_box fix16_boxes[],
                                int boxes_len, const int num_classes);
```

## `post_process_yolo_int8`

Postprocessing for YOLOv2/v3/v4/v5. `yolo_info_t` carries output sizes and anchor configuration.

```c
int post_process_yolo_int8(int8_t **outputs,
                           const int num_outputs,
                           int zero_points[],
                           fix16_t scale_outs[],
                           yolo_info_t *cfg,
                           fix16_t thresh,
                           fix16_t overlap,
                           fix16_box fix16_boxes[],
                           int max_boxes);
```

## `pprint_post_process`

Thin wrapper that dispatches to the correct postprocess routine by name and prints the result.

```c
int pprint_post_process(const char *name,
                        const char *pptype,
                        model_t *model,
                        vbx_cnn_io_ptr_t *o_buffers,
                        int int8_flag,
                        int fps,
                        vbx_cnn_t *the_vbx_cnn);
```

Returns `-1` if the postprocess type is unknown, otherwise `0`. Pass `fps = 1` for simulation runs.

---

Source: [VectorBlox-SDK/docs/postprocess.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/postprocess.md)
