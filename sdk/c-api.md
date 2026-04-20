---
title: C API Reference
layout: default
parent: SDK Overview
nav_order: 5
---

# C API Reference

The C API is shared between the hardware runtime and the host simulator. Functions fall into three buckets: enum types, model queries, and IP-core runtime calls. A minimal Python API for the simulator is documented at the end.

## Enum types

| Enum | Values |
|---|---|
| `vbx_cnn_calc_type_e` | `VBX_CNN_CALC_TYPE_UINT8`, `VBX_CNN_CALC_TYPE_INT8`, `VBX_CNN_CALC_TYPE_INT16`, `VBX_CNN_CALC_TYPE_INT32`, `VBX_CNN_CALC_TYPE_UNKNOWN` |
| `vbx_cnn_size_conf_e` | `VBX_CNN_SIZE_CONF_V250 = 0`, `VBX_CNN_SIZE_CONF_V500 = 1`, `VBX_CNN_SIZE_CONF_V1000 = 2` |
| `vbx_cnn_comp_conf_e` | `VBX_CNN_CONF_NO_COMPRESSION = 0`, `VBX_CNN_CONF_COMPRESSION = 1`, `VBX_CNN_CONF_UNSTRUCTURED_COMPRESSION = 2` |
| `vbx_cnn_err_e` | `START_NOT_CLEAR = 3`, `OUTPUT_VALID_NOT_SET = 4`, `INVALID_NETWORK_ADDRESS = 6`, `MODEL_BLOB_INVALID = 7`, `MODEL_BLOB_VERSION_MISMATCH = 8`, `MODEL_BLOB_CONFIGURATION_MISMATCH = 9` |
| `vbx_cnn_state_e` | `READY = 1`, `RUNNING = 2`, `RUNNING_READY = 3`, `FULL = 6`, `ERROR = 8` |

## Model queries

All take a `const model_t *model` and return information about the compiled graph.

| Function | Returns |
|---|---|
| `int model_check_sanity(const model_t *model)` | Non-zero if the model does not look valid. |
| `size_t model_get_allocate_bytes(const model_t *model)` | Total bytes required to store model + temporary buffers (must be contiguous with model data). |
| `size_t model_get_data_bytes(const model_t *model)` | Bytes required for the data portion only. |
| `size_t model_get_num_inputs(const model_t *model)` | Number of input buffers. |
| `size_t model_get_num_outputs(const model_t *model)` | Number of output buffers. |
| `size_t model_get_input_dims(const model_t *model, int input_index)` | Number of input dimensions at `input_index`. |
| `size_t model_get_output_dims(const model_t *model, int index)` | Number of output dimensions at `index`. |
| `int* model_get_input_shape(const model_t *model, int index)` | Dimensions array for input `index`. |
| `int* model_get_output_shape(const model_t *model, int index)` | Dimensions array for output `index`. |
| `size_t model_get_input_length(const model_t *model, int input_index)` | Length (elements) of the input buffer. |
| `size_t model_get_output_length(const model_t *model, int index)` | Length (elements) of the output buffer. |
| `vbx_cnn_calc_type_e model_get_input_datatype(const model_t *model, int input_index)` | Input data type. |
| `vbx_cnn_calc_type_e model_get_output_datatype(const model_t *model, int output_index)` | Output data type. |
| `float model_get_output_scale_value(const model_t *model, int index)` | Floating-point output scale. |
| `int model_get_output_scale_fix16_value(const model_t *model, int index)` | Output scale in fix16. |
| `int model_get_input_scale_fix16_value(const model_t *model, int index)` | Input scale in fix16. |
| `int model_get_output_zeropoint(const model_t *model, int index)` | Output zero point (fix16 format). |
| `int model_get_input_zeropoint(const model_t *model, int index)` | Input zero point (fix16 format). |
| `vbx_cnn_size_conf_e model_get_size_conf(const model_t *model)` | `0 = V250`, `1 = V500`, `2 = V1000`. |
| `vbx_cnn_comp_conf_e model_get_comp_conf(const model_t *model)` | `0 = No Compression`, `1 = Compression`, `2 = Unstructured Compression`. |
| `void* model_get_test_input(const model_t *model, int index)` | Pointer to embedded test input, if any. |

## IP-core runtime

### `vbx_cnn_init`

```c
vbx_cnn_t* vbx_cnn_init(volatile void *ctrl_reg_addr);
```

Initializes the VectorBlox IP core. Pass the address of the `VBX CNN S_control` port. The returned `vbx_cnn_t` has `.initialized` set on success. Calling any other function with an uninitialized `vbx_cnn_t` is undefined behaviour.

### `vbx_cnn_get_state`

```c
vbx_cnn_state_e vbx_cnn_get_state(vbx_cnn_t *vbx_cnn);
```

Returns the current core state: `READY (1)`, `RUNNING (2)`, `RUNNING_READY (3)`, `FULL (6)`, or `ERROR (8)`.

### `vbx_cnn_get_error_val`

```c
vbx_cnn_err_e vbx_cnn_get_error_val(vbx_cnn_t *vbx_cnn);
```

Returns the current value of the error register.

### `vbx_cnn_model_start`

```c
int vbx_cnn_model_start(vbx_cnn_t *vbx_cnn, model_t *model, vbx_cnn_io_ptr_t *io_buffers);
```

Runs the model. One model can be queued while another is running for peak throughput. Returns non-zero when the model cannot run (state is `FULL` or `ERROR`). `io_buffers` lists input pointers first, then output pointers.

```cpp
vbx_cnn_model_start(vbx_cnn, model, io_buffers);
while (input = get_input()) {
    io_buffers[0] = input;
    vbx_cnn_model_start(vbx_cnn, model, io_buffers);
    while (vbx_cnn_model_poll(vbx_cnn) > 0);
}
while (vbx_cnn_model_poll(vbx_cnn) > 0);
```

### `vbx_cnn_model_poll`

```c
int vbx_cnn_model_poll(vbx_cnn_t *vbx_cnn);
```

Waits for a running model to complete. Returns `1` (running), `0` (done), `-1` (error during processing), or `-2` (no network running).

### `vbx_cnn_model_wfi`

```c
int vbx_cnn_model_wfi(vbx_cnn_t *vbx_cnn);
```

Uses an interrupt to wait for model completion. Returns `1` (running), `0` (done or not running), or `-1` (error).

### `vbx_cnn_model_isr`

```c
void vbx_cnn_model_isr(vbx_cnn_t *vbx_cnn);
```

Interrupt service routine — clears the `output_valid` register once the running model is done.

### `model_check_configuration`

```c
int model_check_configuration(const model_t *model, vbx_cnn_t *vbx_cnn);
```

Returns non-zero if the compiled model does not match the IP-core configuration it is being loaded onto.

### `vbx_tsnp_model_start`

```c
int vbx_tsnp_model_start(vbx_cnn_t *vbx_cnn, model_t *model, model_t *tsnp_model,
                         uint32_t input_offset, vbx_cnn_io_ptr_t io_buffers[]);
```

Runs a model and its TSNP (transpose-softmax-normalization-postprocess) companion with the supplied IO buffers. Usage mirrors `vbx_cnn_model_start`.

## Python simulator API (`vbx.sim.Model`)

### Methods

- `__init__(self, model_bytes)` — creates a model object from raw bytes.
- `run(self, inputs)` — runs the model with a list of NumPy input arrays and returns a list of NumPy output arrays.

### Attributes

- `num_inputs`, `num_outputs` — counts of IO buffers.
- `input_lengths`, `output_lengths` — lengths (in elements) of each buffer.
- `input_dims`, `output_dims` — dimensions of each buffer.
- `input_dtypes`, `output_dtypes` — NumPy dtypes for each buffer.
- `output_scale_factor` — per-output float scale factors.
- `description` — free-form string embedded by the compiler.
- `test_input` — a list of inputs you can pass directly to `Model.run()` for self-test.

---

Source: [VectorBlox-SDK/docs/C_API.md](https://github.com/Microchip-Vectorblox/VectorBlox-SDK/blob/master/docs/C_API.md)
