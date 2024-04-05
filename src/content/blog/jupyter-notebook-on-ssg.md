---
title: Share Jupyter Notebook
author: syamaguc
pubDatetime: 2023-11-01T00:00:00Z
postSlug: share-jupyter-notebook
featured: false
draft: false
tags:
  - dev
  - python
description: Jupyter Notebookをブログで簡単にシェアする
---

Jupyter Notebookの分析結果をWeb上で簡単にシェアできたら便利だなと思って調べてみた。

[nbviewer](https://nbviewer.org/)にnotebookのリンクを張り付け生成されたリンクを`iframe`で埋め込む。

とりあえず、GitHubに落ちていた`kaggle-titanic-analysis`を埋め込んでみた。

<div>
  <iframe src="https://nbviewer.org/github/agconti/kaggle-titanic/blob/master/Titanic.ipynb" width="100%" height="1000px"></iframe>
</div>
