---
title: Jupyter Notebook on SSG
author: syamaguc
pubDatetime: 2023-11-01T00:00:00Z
postSlug: jupyter-notebook-on-ssg
featured: false
draft: false
tags:
  - dev
  - python
  - datascience
description: Jupyter NotebookをSSG製のブログで簡単にシェアする
---

Jupyterの分析をブログで簡単にシェアできたら便利やなと思って調べたら、めちゃ簡単だった。

[nbviewer](https://nbviewer.org/)にnotebookのリンクを張り付け生成されたリンクを`iframe`で埋め込む。

とりあえず、GitHubに落ちていた`kaggle-titanic-analysis`を埋め込んでみた。

これは便利、これから競馬の分析結果とかこれでシェアしていきたい。

<div>
  <iframe src="https://nbviewer.org/github/agconti/kaggle-titanic/blob/master/Titanic.ipynb" width="100%" height="1000px"></iframe>
</div>
