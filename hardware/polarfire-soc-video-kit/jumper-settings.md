---
title: Jumper Settings
layout: default
parent: PolarFire SoC Video Kit
grand_parent: Hardware
nav_order: 2
---

# PolarFire SoC Video Kit Jumper Settings

The PolarFire SoC Video Kit ships with its jumpers already in the correct positions for the VectorBlox demo. Only revisit this page if you've changed the jumpers or the board is not booting as expected.

## Diagram

![PolarFire SoC Video Kit jumper locations]({{ "/assets/images/jumper-settings.png" | relative_url }})

## Required settings

{: .important }
Jumpers **J16** and **J35** must **not** be open. Both must be connected across pins **2–3**.

| Jumper | Required position | Purpose |
|---|---|---|
| **J16** | 2–3 | Boot-mode selection — must be closed on pins 2–3 |
| **J35** | 2–3 | Boot-mode selection — must be closed on pins 2–3 |

All other jumpers should remain in their factory default positions. For the full jumper map (power rails, debug, boot-mode selection, etc.) refer to the [VectorBlox v3.0 PolarFire SoC Video Kit Demo Guide PDF](https://github.com/Microchip-Vectorblox/VectorBlox-SoC-Video-Kit-Demo/blob/main/docs/VectorBlox_PolarFire_SoC_Video_Kit_Demo_Guide.pdf).

## Troubleshooting

| Symptom | Likely jumper issue |
|---|---|
| HSS does not start on UART0 at power-on | **J16** or **J35** is open or set to 1–2 |
| Board powers on but FlashPro Express cannot see the device | Boot-mode jumpers misconfigured — verify **J16** and **J35** |
| Unexpected behavior after swapping job-file variants | Re-check jumpers, then delete `VectorBlox-SDK-release` folders on the eMMC and re-run the quickstart script |

## Next steps

- Return to the [Quickstart]({% link hardware/polarfire-soc-video-kit/quickstart.md %}) to continue with hardware connections.
