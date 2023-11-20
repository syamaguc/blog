---
title: My Keiba AI v1.0.0
author: syamaguc
pubDatetime: 2023-11-20T00:00:00Z
postSlug: keiba-ai-v1
featured: false
draft: true
tags:
  - ai
  - 競馬
description: talk is cheap, show me the code.
---

## Table of Contents

<!-- toc -->

## DB設計

現時点ではあらゆる仮説に対してオープンなため、できるだけ使い易い状態にしておきたい。
無料のDBのみ利用予定のため、netkeiba.comのデータをスクレイピングして取得する。

### Race

#### race_master

- id
- date(YYYY-MM-DD)
- start(出走時刻)
- hold(開催)
- days(x日目)
- location
- distance
- ground_type(芝orダート)
- direction(右or左)
- class
- weather
- ground_state
- ground_index

#### race_shutuba

- id
- waku
- gate
- horse_name
- horse_id
- jockey_name
- jockey_id
- trainer_name
- trainer_name
- belongs(栗東or美浦)
- gender
- age
- handicap
- weight
- weight_diff
- odds
- popularity

#### race_result

- id
- arrival
- horse_id
- time
- remark
- diff
- speed_index
- start_3f
- last_3f
- corner

#### race_lap

#### race_payout

- id
- tansho
- fukusho1
- fukusho2
- fukusho3
- wide1
- wide2
- wide3
- umaren
- umatan
- wakuren
- sanrenpuku
- sanrentan

### Horse

#### race_history

### Location & Course

#### location_master

- id
- distance_in_a_straight(直線距離)
- is_slope(直線の坂の有無)
- type_direction(右・左)

#### course_master

- id
- location_id
- distance
- type_ground(芝・ダート)
- distance_to_the_first_corner(最初のコーナーまでの距離)
