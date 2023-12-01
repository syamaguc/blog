---
title: Experiment - Yesmix & Anna Kyoyama
author: syamaguc
pubDatetime: 2023-11-30T11:30:00Z
postSlug: yesmix-anna-kyoyama
featured: false
draft: true
tags:
  - ai
  - anime
description: Stable DiffusionでYes-MixとシャーマンキングのアンナのLoraを組み合わせてみる。
---

## Table of Contents

<!-- toc -->

## 構図

### パターンA

## Setup

- checkpoint: `Yesmix_v35`
- VAE: `kl-f8-anime2.vae.pt`
- Sampling metod: `DPM++ 2M Karras`
- Sampling steps: `30`
- Refiner: `None`
- Upscaler: `Latent`
- Upscale by: `2`
- Denoising strength: `0.7`
- width: `270`
- height: `480`
- CFG Scale: `7`
- seed: `-1`

### base prompt

```text
(masterpiece:1.4), (best quality:1.1),(ultra detailed:1.1),(highres)
<lora:kyouyama_anna_v1:0.7>,
kaa, yellow eyes, black dress, bead necklace, sleeveless, bracelet,
(cute),
small breasts,
bare shoulders,
```

```text
(negative_hand-neg:1), (easynegative:1),
(bad quality: 1),(worst quality:1),(low quality:1),
normal quality,lowres, lower, bad anatomy,
nsfw,
missing fingers,
bad hands,
(low quality lowres simple background:1.4)
```

### Additional

#### arms behind head

```text
bandana, arms behind head,
upper body,
indoors, things,
```

#### Crossed legs

```text
red scurf, crossed legs,
strapless,things,
```

#### Ecchi

```
bandana, shirt, naked shirt, shirt tug, strapless,
blush,
```

#### cowboy shotrms

```
cowboy shot
```
