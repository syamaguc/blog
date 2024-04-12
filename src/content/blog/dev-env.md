---
title: 開発環境の整理
author: syamaguc
pubDatetime: 2022-04-01T00:00:00Z
modDatetime:
postSlug: dev-env
featured: false
draft: false
tags:
  - dev
description: No mouse, good life
---

Linux, Mac OS, Windowsの異なる環境で出来るだけキーボードのみで操作を完結できるような試行錯誤の記録

1. 必要なツールをインストール
1. [設定ファイル](https://github.com/syamaguc/config)を`stow`でシンボリックリンクを貼れば環境構築が完了する。

|          | Ubuntu           | Mac              | Windows          | WSL2 |
| -------- | ---------------- | ---------------- | ---------------- | ---- |
| IME      | ibus-skk         | Aqua-SKK         | CorvusSKK        |      |
| WM       | i3               | yabai            |                  |      |
| Launcher | rofi             | alfred           |                  |      |
| Terminal | Alacritty + Tmux | Alacritty + Tmux |                  |      |
| Shell    | zsh              | zsh              |                  | zsh  |
| Browser  | Firefox + Vimium | Firefox + Vimium | Firefox + Vimium |      |

## Ubuntu

現時点での最適解は、[Regolith](https://regolith-desktop.com/)をベースに色々とカスタマイズするのが最も楽。

## Mac

最近yabaiが安定してきた。

## Windows

`確定申告`と`TARGET frontier JV`のためだけにあるOSなので、そこまで頑張って環境構築する必要はない。優先順位は低い。
