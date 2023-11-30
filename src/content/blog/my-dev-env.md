---
title: My dev env
author: syamaguc
pubDatetime: 2022-5-11T00:00:00Z
postSlug: my-dev-env
featured: true
draft: true
tags:
  - dev
  - linux
  - mac
  - dotfiles
description: 随時更新 | 自分の開発環境についてのwiki
---

## Table of Contents

<!-- toc -->

## Overview

|          | Arch Linux<br>@Desktop | Arch Linux<br>@Thinkpad | Ubuntu<br>@EC2 | M1 MacOS<br>desktop  | Windows11<br>@Desktop |
| -------- | ---------------------- | ----------------------- | -------------- | -------------------- | --------------------- |
| 用途     | 自宅                   | 外出時                  |                | オフィス             | 確定申告とゲーム      |
| Termnal  | Alacritty<br> + tmux   | Alacritty<br> + tmux    |                | Alacritty<br> + tmux |                       |
| WDM      | i3                     | i3                      |                | yabai                |                       |
| Editor   | Neovim                 | Neovim                  | vim            | Neovim               |                       |
| Shell    | ZSH                    | ZSH                     | ZSH            | ZSH                  |                       |
| Launcher | rofi                   | rofi                    |                | alfread              |                       |
| Storage  | Dropbox                | Dropbox                 |                | Dropbox              |                       |

## Setup

```makefile
COMMON = git zsh vim
LOCAL_COMMON = gh lazygit nvim tmux alacritty bin ranger
LINUX = x i3 i3blocks picom rofi conky libskk dunst
MAC_OS = yabai skhd

archlinux: local
	@stow -v $(LINUX)

mac: local aquaskk
	@stow -v $(MAC_OS)

local: server
	@stow -v $(LOCAL_COMMON)

server:
	@stow -v $(COMMON)

clean:
	@stow -Dv */

```

## Tools

### Terminal

#### Alacritty

termialに表示されたURLを開く際にデフォルトで`private-mode`を利用する。

```yml
hints:
  enabled:
    - regex: "(https:|http:)[^\u0000-\u001F\u007F-\u009F<>\"\\s{-}\\^⟨⟩`]+"
      command: { program: "google-chrome-stable", args: ["--incognito"] }
      post_processing: true
      mouse:
        enabled: true
```

#### tmux

sessionやwindow間の移動は基本的に全て`choose-tree`を使うことで、余計なキーバインドを覚えなくて済むし混乱しない。

自分の場合は`<C-a> <Space>`に割り当てている。

leader = <C-a>

| key                   | action            |
| --------------------- | ----------------- |
| leader + \<space\>    | choose-tree       |
| leader + :            | command line mode |
| leader + l            | next window       |
| leader + ,            | rename window     |
| leader + \<c-[hjkl]\> | move pane [hjkl]  |
| tls                   | tmux ls           |
| ta                    | tmux attach       |
| tkill                 | tmux kill         |
| tnew                  | tmux new-session  |

### WDM

#### i3 & Yabai

- よく使うキーバインド

| key                      | i3<br>@archlinux                     | Yabai<br>@MacOS |
| ------------------------ | ------------------------------------ | --------------- |
| SUPER-L + [1-9]          | move workspace [1-9]                 | =               |
| SUPER-L + shift + [1-9]  | move focus-window to workspace [1-9] | =               |
| SUPER-L + [hjkl]         | move focus to [hjkl]                 | =               |
| SUPER-L + shift + [hjkl] | swap focus-window to [hjkl]          | =               |
| SUPER-L + shift + r      | reload WDM                           | =               |
| SUPER-L + shift + d      | quit focus-window                    | =               |
| ALT + \<space\>          | launch rofi                          | launch Alfread  |
| ALT + shift + \<space\>  | search window with rofi              |                 |
| ALT + shift + q          | system menue with rofi               |                 |
| SUPER-L + p              | screenshot with scrot                |                 |
| SUPER-L + shift + p      | screenshot with gyazo                |                 |

自宅と職場で別のOSを使っている(Linux, macOS)こともあり、
出来るだけ開発環境を統一すべく試行錯誤した。
極力マウスを使いたくない自分としては、
Macでのタイル型ウィンドウマネージャがずっと定まらなくて困っていたが、
やっとYabaiが安定してきたので、本格的に導入した。
いまのところ、自宅でもオフィスでもストレス無く開発出来ている。

### Browser

#### Chrome Extensions

##### Vimium

新しいタブを開くと、デフォルトの設定だとVomnibarの操作が効かなくなるのが不便だったので
[Tabページ](https://syamaguc.vercel.app/)を作って新規タブの遷移先を変更。
Themeを[Frappe](https://github.com/catppuccin/vimium/blob/main/dist/catppuccin-vimium-frappe.css)にすると、
ChromeのThemeが[Dracula Chrome Theme](https://draculatheme.com/chrome)なので色合いがちょうど良い。

```text
##### re map keybind #####

# go & back
map H goBack
map L goForward
map <c-h> goBack
map <c-l> goForward

# new tab action
map t Vomnibar.activateTabSelection
map T createTab

# move tab
map [ previousTab
map ] nextTab

# moving
map J scrollFullPageDown
map K scrollFullPageUp
map <c-j> scrollFullPageDown
map <c-k> scrollFullPageUp
map h scrollLeft
map l scrollRight
```

##### Gyazo

基本画像やScreenshotは`gyzo`でクラウドに保存する
