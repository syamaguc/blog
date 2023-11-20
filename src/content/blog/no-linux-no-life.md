---
title: No Linux, No life
author: syamaguc
pubDatetime: 2022-5-11T00:00:00Z
postSlug: no-linux-no-life
featured: true
draft: false
tags:
  - dev
  - linux
  - mac
description: 随時更新 | 自分の開発環境についてのwiki
---

<div align="center">
  <a href="https://i.gyazo.com/37dd53120afc03c3a9ebf0daf3780b6e">
    <img src="https://i.gyazo.com/37dd53120afc03c3a9ebf0daf3780b6e.png" alt="Image from Gyazo" width="500"/>
  </a>
</div>

## Table of Contents

## Overview

|          | Arch Linux<br>@Desktop | M1 MacOs<br>@Desktop | Arch Linux<br>@Thinkpad | Ubuntu<br>@EC2    | Windows11<br>@Desktop |
| -------- | ---------------------- | -------------------- | ----------------------- | ----------------- | --------------------- |
| 用途     | 自宅                   | オフィス             | 外出時                  | cron, 緊急時, etc | 確定申告とゲーム      |
| Termnal  | Alacrity<br> + tmux    | =                    | =                       |                   |                       |
| WDM      | i3                     | Yabai                | i3                      |                   |                       |
| Editor   | Neovim                 | =                    | =                       | vim               |                       |
| Shell    | ZSH                    | =                    | =                       | =                 |                       |
| Launcher | rofi                   | Alfread              | rofi                    |                   |                       |
| Storage  | Dropbox                | =                    | =                       |                   |                       |

- [my dotfiles](https://github.com/syamaguc/config)

```makefile
COMMON = git zsh vim
LOCAL_COMMON = lazygit nvim tmux alacritty bin
LINUX = x i3 i3blocks picom rofi conky libskk ranger dunst
MAC_OS = yabai skhd

archlinux: local
	@stow -v $(LINUX)

mac: local
	@stow -v $(MAC_OS)

# local common setup
local: server
	@stow -v $(LOCAL_COMMON)

# for server minimal setup, ec2, gce, etc
server:
	@stow -v $(COMMON)


clean:
	@stow -Dv */
```

## tool

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

| key                    | action            |
| ---------------------- | ----------------- |
| \<C-a\> + \<space\>    | choose-tree       |
| \<C-a\> + :            | command line mode |
| \<C-a\> + l            | next window       |
| \<C-a\> + \<c-[hjkl]\> | move pane [hjkl]  |
| \<C-a\> + ,            | rename window     |
| tls                    | tmux ls           |
| ta                     | tmux attach       |
| tkill                  | tmux kill         |
| tnew                   | tmux new-session  |

### WDM

#### i3 & Yabai

| key                      | i3<br>@archlinux                     | Yabai<br>@MacOS |
| ------------------------ | ------------------------------------ | --------------- |
| SUPER-L + [1-9]          | move workspace [1-9]                 | =               |
| SUPER-L + shift + [1-9]  | move focus-window to workspace [1-9] | =               |
| SUPER-L + [hjkl]         | move focus to [hjkl]                 | =               |
| SUPER-L + shift + [hjkl] | swap focus-window to [hjkl]          | =               |
| SUPER-L + shift + r      | reload WDM                           | =               |
| SUPER-L + shift + d      | quit focus-window                    | ???             |
| ALT + \<space\>          | launch rofi                          | launch Alfread  |
| ALT + shift + \<space\>  | search window with rofi              | ???             |
| ALT + shift + q          | system menue with rofi               | ???             |
| SUPER-L + p              | screenshot with scrot                | ???             |
| SUPER-L + shift + p      | screenshot with gyazo                | ???             |

自宅と職場で別のOSを使っていることもあり、

出来るだけ開発環境を統一すべく試行錯誤した。

極力マウスを使いたくない自分としては、

Macでのタイル型ウィンドウマネージャがずっと定まらなくて困っていたが、

やっとYabaiが安定してきたので、本格的に導入した。

いまのところ、自宅でもオフィスでもストレス無く開発出来ている。

[![Image from Gyazo](https://i.gyazo.com/7e96b3ea3ed5465813d67dc784b6a908.png)](https://gyazo.com/7e96b3ea3ed5465813d67dc784b6a908)

### Browser

#### Chrome

新しいタブを開くと、デフォルトの設定だとVomnibarの操作が効かなくなるのが不便だったので

[NewTabページ](https://syamaguc.vercel.app/)を作って新規タブの遷移先を変えた。

あとThemeを[Frappe](https://github.com/catppuccin/vimium/blob/main/dist/catppuccin-vimium-frappe.css)にしたら、

ChromeのThemeが[Dracula Chrome Theme](https://draculatheme.com/chrome)なので、色合いがちょうど良い。

[![Image from Gyazo](https://i.gyazo.com/271f68b392d3a8c873c3f6c7ca7da2cd.png)](https://gyazo.com/271f68b392d3a8c873c3f6c7ca7da2cd)

<br>

[![Image from Gyazo](https://i.gyazo.com/2244051648ec352150e0cfa1a31ed2c9.png)](https://gyazo.com/2244051648ec352150e0cfa1a31ed2c9)

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

### Other

#### Gyazo

Screenshotは`gyzo`でクラウドに保存する

## tips

### cron

serverの`timezone`を変更した場合cronを再起動するのを忘れずに。

```bash
sudo timedatectl set-timezone Asia/Tokyo
sudo systemctl restart cron
```

`format`は以下の通り。

```bash
# minute hour day_of_month month day_of_week command
# minute は 0 から 59 までの値。
# hour は 0 から 23 までの値。
# day_of_month は 1 から 31 までの値。
# month は 1 から 12 までの値。
# day_of_week は 0 から 6 までの値、0 が日曜日。

# ex) `myscript.sh`をsummer time (6月, 7月, 8月) を除く平日(月-金)の午前9時から午後4:55まで5分間隔で実行
*/5 9-16 * 1-5,9-12 1-5 ~/bin/myscript.sh

# @reboot : 起動時
# @yearly : 一年毎
# @annually ( == @yearly)
# @monthly : 一月毎
# @weekly : 一周毎
# @daily : 一日毎
# @midnight ( == @daily)
# @hourly : 一時間毎

@reboot ~/bin/myscript.sh

```

### USB

- [archlinux wiki - USB Storage Device](https://wiki.archlinux.jp/index.php/USB_%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B8%E3%83%87%E3%83%90%E3%82%A4%E3%82%B9)

`lsblk -f | grep "sd[a-z]"` とかでデバイスの`uuid`を確認

```bash
#!/bin/bash

sudo mkdir -p /mnt/usb

if [ "$1" == "mount" ]; then
	sudo mount /dev/disk/by-uuid/"$2" /mnt/usb -o umask=000
fi

if [ "$1" == "remove" ]; then
	sudo umount /mnt/usb
fi
```

## 設定関連

### GPU

ディスプレイマネージャーを変えたり、ドライバーをアップデートするとたまに問題が起きる。
とりあえず、これ読めば大抵は解決する。

- [archlinux wiki - NVIDIA](https://wiki.archlinux.jp/index.php/NVIDIA)

### Windowsとのデュアルブート

自宅のデスクトップPCはArchlinuxとWindows11のデュアルブート環境を利用している。何故なら、Windowsがないと確定申告が出来ないから。(e-tax)

毎度Windows Updateの度に起きる問題、結論`Windows側で高速スタートアップを無効化`

- [archlinux wiki - WindowsとArchのデュアルブート](https://wiki.archlinux.jp/index.php/Windows_%E3%81%A8%E3%81%AE%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%96%E3%83%BC%E3%83%88)
  - [高速スタートアップとハイバーネート](https://wiki.archlinux.jp/index.php/Windows_%E3%81%A8%E3%81%AE%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%96%E3%83%BC%E3%83%88#.E9.AB.98.E9.80.9F.E3.82.B9.E3.82.BF.E3.83.BC.E3.83.88.E3.82.A2.E3.83.83.E3.83.97.E3.81.A8.E3.83.8F.E3.82.A4.E3.83.90.E3.83.8D.E3.83.BC.E3.83.88)

ついでに時刻関連の設定も。自分はLinux側でUTCを採用しているので、Windowsが起動時に勝手にハードウェアクロックにメスを入れてしまうという状況が起きる。

- [WindowsでUTCを使う](https://wiki.archlinux.jp/index.php/%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E6%99%82%E5%88%BB#Windows_.E3.81.A7_UTC_.E3.82.92.E4.BD.BF.E3.81.86)

> ハードウェアクロック (CMOS クロック, BIOS で表示される時間) で使われる時刻系はオペレーティングシステムによって定義されます。デフォルトでは、Windows は localtime を使いますが、Mac OS は UTC を使っていて、UNIX ライクなオペレーティングシステムでは決まっていません。
