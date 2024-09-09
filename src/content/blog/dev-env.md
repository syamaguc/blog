---
title: Arch Linuxを使う
author: syamaguc
pubDatetime: 2022-04-01T00:00:00Z
modDatetime: 2024-07-07T00:00:00Z
postSlug: dev-env
featured: false
draft: false
tags:
  - dev
description: 軽量、シンプル、ミニマル。不要なもの、余計なものは入れない。
---

## はじめに

Linux, Mac OS, Windowsの異なる環境で出来るだけキーボードのみで操作を完結できるような試行錯誤の記録

1. 必要なツールをインストール(`make install`)
1. [設定ファイル](https://github.com/syamaguc/config)を`stow`でシンボリックリンクを貼れば環境構築が完了する。

という理想状態を目指している。

Makefileを使って、環境のセットアップスクリプトを構築している人は意外と見ないが、依存関係の解決が楽なので親和性が高いと思っている。(Aをインストールしていないと、Bをインストール出来ない等)

## ツール選定

|          | Arch Linux       | Mac              | Windows          | WSL2 |
| -------- | ---------------- | ---------------- | ---------------- | ---- |
| IME      | ibus-skk         | Aqua-SKK         | CorvusSKK        |      |
| WM       | i3               | yabai            |                  |      |
| Launcher | rofi             | alfred           |                  |      |
| Terminal | Alacritty + Tmux | Alacritty + Tmux |                  |      |
| Shell    | zsh              | zsh              |                  | zsh  |
| Browser  | Firefox + Vimium | Firefox + Vimium | Firefox + Vimium |      |

## 各コメント(随時更新)

### Arch Linux

- 定期的なライブラリのアップデートを怠ると、keyringの問題が発生する。`sudo pacman -Sy archlinux-keyring`で解決する。(2023/03/05)

### Mac

- 最近yabaiが安定してきた(2023/12/15)

### Windows

- タイル型WMを導入したいが、`確定申告`と`TARGET frontier JV`のためだけにあるOSなので優先度低い。
