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
description: 随時更新 | Linuxについてのmemo
---

## Table of Contents

## General

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

## Hardware

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
