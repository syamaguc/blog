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
description: Linuxについてのtips & memo, 随時更新
---

<div align="center">
  <a href="https://i.gyazo.com/37dd53120afc03c3a9ebf0daf3780b6e">
    <img src="https://i.gyazo.com/37dd53120afc03c3a9ebf0daf3780b6e.png" alt="Image from Gyazo" width="500"/>
  </a>
</div>

### GPU関連

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
