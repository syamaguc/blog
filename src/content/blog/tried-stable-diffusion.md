---
title: Black magic in Stable Diffusion
author: syamaguc
pubDatetime: 2023-11-27T03:00:00Z
postSlug: tried-stable-diffusion
featured: false
draft: false
tags:
  - dev
  - ai
  - stablediffusion
description: AUTOMATIC1111/Stable-Diffusion-webUI 試してみたのでメモ
---

## Table of Contents

## Model

- checkpoint: ベースモデル
- refiner: 追加学習モデル1
- lora: 追加学習モデル2

くらいの認識で良いかと。以下のサイトからモデルを選び追加する。`civitai`はサンプルイメージがあるので、最初は分かりやすいかも。
レポジトリの`models/Stable-diffusion/{model}`にモデルを追加したら、Stable Diffusion Checkpointから選択できるようになる。`Rifiner`も同様の場所で、`Lora`は`models/Lora/{model}`、

- [civitai](https://civitai.com/)
- [huggingface](https://huggingface.co/)

## Prompt

### danboru tag

モデルによっては、[danboru](https://danbooru.donmai.us/)の画像とタグから学習しているものがあるらしく、(NovelAI, Waifu Diffusion etc)、
そういったモデルに対しては、効果的とのこと。

その他tipsとしては、

- `Booru tag autocompletion for A1111`をインストールするとタグ補完してくれる。

- `img2img`で`Interrrogate DeepBooru`を押すと`imt2txt`してくれる。

### Negative Prompt

基本的に反映したくない要素をいれとく。

## Parameters

### Sampling method

WIP

### sampling Steps

画像のノイズを除去する回数を指す。
このパラメータ値が高ければ高いほど、質の高い画像を生成できる反面、計算時間がかかる。
基本デフォルトで、構図が固まったら30-40くらいにするのが良いか？

### Hires. fix

大きな画像を出力する際に、工程を2段階に分けることで構図の破綻を抑えようという機能。

#### Hires steps

WIP

#### Denoising strength

WIP

#### Upscale by

WIP

#### Switch at

大きいほど、ベースモデルの効果が強くなり、小さいほど、`Refiner`モデルの効果が強くなる?

### CFG Scale

promptのスケール、大きいほど`prompt`の効果が強くなる。

### Batch count

WIP

### Batch Size

一回の生成で出力する画像の枚数。メモリの使用量に影響するのでスペック次第

## Example

### Asuna

- checkpoint: `koji_V21`
- refiner: `sudachi_V10`
- sampling steps: `20`
- denoising strength: `0.77`
- switch at: `0.35`
- size: `512*512`
- upscale: `2`
- CFG scale: `7`

[![Image from Gyazo](https://i.gyazo.com/590d371fd16f4dad22869e9e89496d48.png)](https://gyazo.com/590d371fd16f4dad22869e9e89496d48)

- checkpoint: `meinamix_meinaV11`
- refiner: `meinaunreal_V41`
- sampling steps: `20`
- denoising strength: `0.77`
- switch at: `0.35`
- size: `512*512`
- upscale: `2`
- CFG scale: `7`

[![Image from Gyazo](https://i.gyazo.com/7098299ac2dfcb6dc4c74ac6a20fadd9.png)](https://gyazo.com/7098299ac2dfcb6dc4c74ac6a20fadd9)

### 人造人間18号

- checkpoint: `meinamix_meinaV11`
- refiner: `meinaunreal_V41`
- sampling steps: `20`
- denoising strength: `0.77`
- switch at: `0.35`
- size: `512*512`
- upscale: `2`
- CFG scale: `7`

[![Image from Gyazo](https://i.gyazo.com/44ae54bfe768a91e78924b1c44fe39b1.png)](https://gyazo.com/44ae54bfe768a91e78924b1c44fe39b1)

### 惣流・アスカ・ラングレー

- checkpoint: `sudachi_V10`
- refiner: `meinamix_meinaV11`
- sampling steps: `25`
- denoising strength: `0.77`
- switch at: `0.77`
- size: `512*512`
- upscale: `2`
- CFG scale: `7`

[![Image from Gyazo](https://i.gyazo.com/d6423b7a8a65c310aaf9a3e7b39b458f.png)](https://gyazo.com/d6423b7a8a65c310aaf9a3e7b39b458f)

### 御坂美琴

[![Image from Gyazo](https://i.gyazo.com/e888921475f52edfae7726d92898fcd6.png)](https://gyazo.com/e888921475f52edfae7726d92898fcd6)

### 灰原哀

[![Image from Gyazo](https://i.gyazo.com/3f2d09bb1befd56257adcb6d96746f07.png)](https://gyazo.com/3f2d09bb1befd56257adcb6d96746f07)

### 錦木千束

[![Image from Gyazo](https://i.gyazo.com/51a0d6cd52355f61eecfa73471a7453b.png)](https://gyazo.com/51a0d6cd52355f61eecfa73471a7453b)

### 井ノ上たきな

[![Image from Gyazo](https://i.gyazo.com/835d04a0ad4409f15b6f44bd1e908f09.png)](https://gyazo.com/835d04a0ad4409f15b6f44bd1e908f09)
