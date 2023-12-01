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

## Models

### Checkpoint

`ベースモデル`の認識。

以下のサイトからモデルを選び追加する。`civitai`はサンプルイメージがあるので、最初は分かりやすいかも。
レポジトリの`models/Stable-diffusion/{model}`にモデルを追加したら、Stable Diffusion Checkpointから選択できるようになる。`Rifiner`も同様の場所で、`Lora`は`models/Lora/{model}`、

- [civitai](https://civitai.com/)
- [huggingface](https://huggingface.co/)

### Refiner

- `途中工程のモデル`みたいな認識
- 例えば`switch_at`が`0.8`の場合、80%の進捗で画像生成が、`Base -> Refiner`に▼切り替わる、みたいなイメージ。

### Lora

- `追加学習ファイル`汎用ではなく、特定のキャラクター特化とか、特定のシチュエーション特化のファイル
- これは理想の画像を生成するためには必須

### VAE

- 少しぼやけたような画像を鮮明化してくれる、くらいの認識。
- 本家の `Stable AI`が公開している汎用のVAE`vae-ft-mse-840000-ema-pruned` がHugging Faceから入手可能。

### Embedding

- `EasyNegative`
- `NegativeHand`
  - ネガティブプロンプトを省略できるくらいの認識

## Prompt

### danboru tag

- 海外の違法？サイト
- モデルによっては、[danboru](https://danbooru.donmai.us/)の画像とタグから学習しているものがあるらしく、(NovelAI, Waifu Diffusion etc)そういったモデルに対しては、効果的らしい。

### Extension

- `prompt presets`で過去のpromptを保存しておかないと、やってられん。
- `Booru tag autocompletion for A1111`をインストールするとタグ補完してくれる。
- `img2img`で`Interrrogate DeepBooru`を押すと`imt2txt`してくれる。

### Negative Prompt

基本的に反映したくない要素をいれとく。

## Parameters

### Sampling method

- モデルの推奨するものを使う
- 特になければ`DPM++ 2M Karras` or `DDIM`
- 大まかな分類をすると、以下のような感じ。
  - Sampling steps 20である程度画像が収束するメソッド
  - Sampling steps 20~40にかけて画像が変化するメソッド

### sampling Steps

- 画像のノイズを除去する工程の回数を指す。
- このパラメータ値が高ければ高いほど、質の高い画像を生成できる反面、計算時間がかかる。
- 基本デフォルト(20)で、構図が固まったらちょい増やす(25くらい)にするのが良いか？

### CFG Scale

promptのスケール、大きいほど`prompt`の効果が強くなる。

### Hire.fix

#### Upscale by

- `Latent`
- アニメ系なら、`R-ESRGAN-4x+-Anime6B`もいい
- リアルなら、`R-ESRGAN-4x+`

#### Hires steps

- `0`でおけ、たぶんだけど、`sampling steps`の値になる。

#### Denoising strength

- 大きいほど、upscaleの際にノイズを除去するが、元画像から変化しやすくなる。

### Size & Batch

#### width \* height

- 基本的にAIが512\*512で学習しているので、これがよいのではないか。
- 大きい画像を生成したい場合、上記くらいのサイズで生成してから
  - Upscaleする
  - igm2imgで拡大する

#### Batch count, size

- `Count`: 逐次実行で、複数枚の画像を生成する。

- `Size`: 並行処理なので、速いがGPUに負担がかかる。並みスペックなら基本使わない。

- BatchCount=2, BatchSize=2の場合、2プロセスで計4枚の画像を生成する。

## Automation Process

- APIを使い、適切なパラメーターを渡すことで、GUIを使わずに画像生成可能。
  - [sample script](https://gist.github.com/w-e-w/0f37c04c18e14e4ee1482df5c4eb9f53)

```python
from datetime import datetime
import urllib.request
import base64
import json
import time
import os

webui_server_url = 'http://127.0.0.1:7861'

out_dir = 'api_out'
out_dir_t2i = os.path.join(out_dir, 'txt2img')
os.makedirs(out_dir_t2i, exist_ok=True)


def timestamp():
    return datetime.fromtimestamp(time.time()).strftime("%Y%m%d-%H%M%S")


def encode_file_to_base64(path):
    with open(path, 'rb') as file:
        return base64.b64encode(file.read()).decode('utf-8')


def decode_and_save_base64(base64_str, save_path):
    with open(save_path, "wb") as file:
        file.write(base64.b64decode(base64_str))


def call_api(api_endpoint, **payload):
    data = json.dumps(payload).encode('utf-8')
    request = urllib.request.Request(
        f'{webui_server_url}/{api_endpoint}',
        headers={'Content-Type': 'application/json'},
        data=data,
    )
    response = urllib.request.urlopen(request)
    return json.loads(response.read().decode('utf-8'))


def call_txt2img_api(**payload):
    response = call_api('sdapi/v1/txt2img', **payload)
    for index, image in enumerate(response.get('images')):
        save_path = os.path.join(out_dir_t2i, f'txt2img-{timestamp()}-{index}.png')
        decode_and_save_base64(image, save_path)



if __name__ == '__main__':
    model = ""
    vae = ""
    sampler = ""
    upscaler = ""
    for _ in range(10):
      payload = {
          "prompt": """
          """,
          "negative_prompt": """
          """,
          "seed": -1,
          "steps": 25,
          "width": 270,
          "height": 360,
          "cfg_scale": 7,
          "sampler_name": f"{sapmler}",
          "n_iter": 1,
          "batch_size": 1,
          "enable_hr": True,
          "hr_upscaler": f"{}",
          "hr_scale": 2,
          "denoising_strength": 0.7,
          "clip_skip": 2,
          "override_settings": {
              'sd_model_checkpoint': f"{model}",
              'sd_vae': f"{vae}",
          },
      }
      call_txt2img_api(**payload)


```
