---
id: maixcam
title: MaixCam
---

# MaixCam

MaixCam is a hardware-integrated AI camera from Sipeed. PicoClaw provides native support for running on MaixCam hardware.

## Hardware

- [MaixCAM](https://www.aliexpress.com/item/1005008053333693.html) — ~$50, AI camera with display
- [MaixCAM2](https://www.kickstarter.com/projects/zepan/maixcam2-build-your-next-gen-4k-ai-camera) — 4K AI camera

## Configuration

```json
{
  "channels": {
    "maixcam": {
      "enabled": true,
      "host": "0.0.0.0",
      "port": 18790,
      "allow_from": []
    }
  }
}
```

## Run

```bash
picoclaw gateway
```

:::note
Full MaixCam documentation is available in Chinese in the repository at `docs/channels/maixcam/README.zh.md`.
:::
