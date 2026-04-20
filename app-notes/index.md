---
title: App Notes
layout: default
nav_order: 7
has_children: true
permalink: /app-notes/
---

# Application Notes

Application notes demonstrate real-world use cases that go beyond the standard SDK tutorials. Each note covers a complete, deployable example built on top of the VectorBlox SDK and the PolarFire SoC Video Kit.

## Available application notes

| Page | Description |
|---|---|
| [Face Recognition Demo]({% link app-notes/face-recognition-demo.md %}) | A two-model pipeline (detection + recognition) that identifies faces by name in a live video stream |
| [Updating the Face Recognition Database]({% link app-notes/face-recognition-database.md %}) | How to build a custom face database from your own images and integrate it into the demo |

## Prerequisites

All application notes assume:

- The VectorBlox SDK is installed and the `vbx_env` virtual environment is active. See [Getting Started]({% link getting-started/index.md %}).
- The PolarFire SoC Video Kit is set up with the VectorBlox demo running. See [Hardware]({% link hardware/index.md %}).
