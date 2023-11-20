---
title: No vim, No life
author: syamaguc
pubDatetime: 2022-5-11T00:00:00Z
postSlug: no-vim-no-life
featured: true
draft: false
tags:
  - dev
  - vim
description: 随時更新 | vimの設定, tips, plugin等の変遷ついてlog
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

#### packer.nvim

nvim初期に利用。

#### plug.vim

vim時代に利用。

### Utils

#### skkeleton

日本語入力をskkに統一するため、skkeletonを導入。
これまでIMEを切替えるために、`<Ctrl + Space>`と`<ESC>`をひたすら叩いてきたので、そのクセが抜けるまで時間がかかりそう。
また現状の入力モードを把握するために`skkeleton_indicator`を導入

## Editing

### Keybinding

#### leader key

`<Space>`をLeaderに設定、理由は英字キーボードだと最も叩きやすいから。

#### smart_quit

- 単一バッファは`<leader> bd`(buffer delete)で閉じる。
- 複数ファイルを開いて編集していない時に`<leader> bd`は面倒なので、`smart_quit`を実装。

```lua
--- smart quit
local smart_quit = function()
  local bufnr = vim.api.nvim_get_current_buf()
  local modified = vim.api.nvim_buf_get_option(bufnr, "modified")
  if modified then
    vim.ui.input({
      prompt = "You have unsaved changes.(w/q) ",
    }, function(input)
      if input == "w" then
        vim.cmd("w")
        vim.cmd("q")
      elseif input == "q" then
        vim.cmd("q!")
      end
    end)
  else
    vim.cmd("q!")
  end
end
```

## Source

参考にした記事など

- [Seven habits of effective text editing by Bram Moolenaar](https://www.moolenaar.net/habits.html)
- [Mastering the Vim Language by Chris Toomey](https://www.youtube.com/watch?si=ftR0wOchN5KdbL68&v=wlR5gYd6um0&feature=youtu.be)
- [Practical Vim by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/)
- [Your problem with Vim is that you dont't grok vi](https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118)
- [Vim Text Objects: The Definitive Guide by Jared Carroll](https://blog.carbonfive.com/vim-text-objects-the-definitive-guide/)
- [Vim Registers: Basic and Beyond](https://www.brianstorti.com/vim-registers/)
- [Colder Quickfix Lists](https://vimways.org/2018/colder-quickfix-lists/)
