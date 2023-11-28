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

以下のサイトからモデルを選び追加する。
`civitai`はサンプルイメージがあるので、最初は分かりやすいかも。

- [civitai](https://civitai.com/)
- [huggingface](https://huggingface.co/)

レポジトリの`models/Stable-diffusion/{model}`にモデルを追加したら、Stable Diffusion Checkpointから選択できるようになる。

## Prompt

### danboru tag

モデルによっては、[danboru](https://danbooru.donmai.us/)の画像とタグから学習しているものがあるらしく、(NovelAI, Waifu Diffusion etc)、
そういったモデルに対しては、効果的とのこと。

その他tipsとしては、

- `Booru tag autocompletion for A1111`をインストールするとタグ補完してくれる。

- `img2img`で`Interrrogate DeepBooru`を押すと、imt2txtでタグを予測してくれる。

### Negative Prompt

基本的に反映したくない要素をいれとく。

ex, low_quality etc

## Parameters

### Sampling method

使用するモデルがデフォルトで推奨するものを使う。

### Restore faces

顔補正のオプション、リアル寄りならつけたほうが良いが、アニメイラストの場合はOFF推奨(個人的所感)

### Tiling

上下左右に繋ぎ目無く、綺麗に繋げられる画像を出力するためのオプション。テクスチャや模様を生成したい場合にのみ利用します。風景画や人物画像を出力する際などにはOFF推奨。

### Hires.fix

大きな画像を出力する際に、工程を2段階に分けることで構図の破綻を抑えようという機能。基本的にON推奨。

### sampling Steps

画像のノイズを除去する回数を指す。
このパラメータ値が高ければ高いほど、質の高い画像を生成できる反面、計算時間がかかる。
基本デフォルトで、構図が固まったら30-40くらいにするのが良いか？

### Size

img2imgの場合は、元画像のサイズと合わせるのが良い

### Batch count

一枚の画像生成行う回数。メモリの使用量に影響しない

### Batch Size

一回の生成で出力する画像の枚数。メモリの使用量に影響する

### CFG Scale

promptのスケール。大きいほど、promptの効果が強くなる。

## img2img

WIP
