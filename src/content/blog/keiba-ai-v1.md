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

### Race

#### race_master

#### race_result

#### race_lap

#### race_payout

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
