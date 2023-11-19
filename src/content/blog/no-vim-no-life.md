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

## 2023

### lazy.nvim

きっかけは`reddit`のスレッド, [「lazy vs packer」](https://www.reddit.com/r/neovim/comments/11d1wjm/lazy_vs_packer/)

正直自宅のデスクトップで使っている分には、起動速度とかプラグインの重さはそんなに気にならないのだけど、
`Thinkpad`くらいのスペックだともっさり感じることは多いから、変えて良かったかなと思う。
また、同時に`Lazyvim`というフレームワークも試し中。
設定がデフォルトで自分が必要な機能の大部分を網羅出来るのも良い。

### skkeleton

日本語入力をskkに統一するため、skkeletonを導入。
これまでIMEを切替えるために、`<Ctrl + Space>`と`<ESC>`をひたすら叩いてきたので、そのクセが抜けるまで時間がかかりそう。
また現状の入力モードを把握するために`skkeleton_indicator`を導入

## 2021

### neovim

- `vim` -> `neovim`

に移行、同時に設定ファイルを`lua`に移行

### smart_quit

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

## 2019

### vim

`vim`と出会う

### leader key

`<Space>`をLeaderに設定、理由は英字キーボードだと最も叩きやすいから。
