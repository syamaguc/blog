---
title: No vim, No life
author: syamaguc
pubDatetime: 2022-5-11T00:00:00Z
postSlug: no-vim-no-life
featured: true
draft: true
tags:
  - dev
  - vim
description: 随時更新 | vim, nivmについてのmemo,tips, plugin, etc
---

## Table of Contents

## General

### Plugin Manager

#### lazy.nvim

きっかけは`reddit`のスレッド, [「lazy vs packer」](https://www.reddit.com/r/neovim/comments/11d1wjm/lazy_vs_packer/)

正直自宅のデスクトップで使っている分には、起動速度とかプラグインの重さはそんなに気にならないのだけど、
`Thinkpad`くらいのスペックだともっさり感じることは多いから、変えて良かったかなと思う。
また、同時に`Lazyvim`というフレームワークも試し中。
設定がデフォルトで自分が必要な機能の大部分を網羅出来るのも良い。

vim時代に利用。

### Utils

#### skkeleton

日本語入力をskkに統一するため、skkeletonを導入。
これまでIMEを切替えるために、`<Ctrl + Space>`と`<ESC>`をひたすら叩いてきたので、そのクセが抜けるまで時間がかかりそう。
また現状の入力モードを把握するために`skkeleton_indicator`を導入

## Source

過去参考にした記事など

- [Seven habits of effective text editing by Bram Moolenaar](https://www.moolenaar.net/habits.html)
- [Mastering the Vim Language by Chris Toomey](https://www.youtube.com/watch?si=ftR0wOchN5KdbL68&v=wlR5gYd6um0&feature=youtu.be)
- [Practical Vim by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/)
- [Your problem with Vim is that you dont't grok vi](https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118)
- [Vim Text Objects: The Definitive Guide by Jared Carroll](https://blog.carbonfive.com/vim-text-objects-the-definitive-guide/)
- [Vim Registers: Basic and Beyond](https://www.brianstorti.com/vim-registers/)
- [Colder Quickfix Lists](https://vimways.org/2018/colder-quickfix-lists/)
