---
title: My Zen of Python
author: syamaguc
pubDatetime: 2021-11-05T00:00:00Z
postSlug: my-zen-of-python
featured: true
draft: false
tags:
  - dev
  - python
description: 随時更新 | pythonに関する知見まとめ
---

## Table of Contents

<!-- toc -->

## Zen

```python
import this
```

```text
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

## Library

### pathlib

- `/`を気にしなくていい。

```python
from pathlib import Path

db_dir = "./db" # 左端に/をつけていない
yymm = f"{year}/{month}/" # 左端に/をつけている

filepath = Path(db_dir, yymm, "yymm.json")


```

```python

# read
Path(filepath).read_text()

# write
data = "hogehoge"
Path(filepath).write_text(data)

# with句
with Path(filepath).open() as f:
    data = f.read()
```

- globの処理が楽

```python
dir = "./db"
filepaths = [p for p in Path(dir).glob("*")]
```

### namedtuple

- ex, 自作のファイルシシテムベースのKVSなど

```python
# ファイルシステムベースのKVSの例
import gzip
import pickle
from pathlib import Path
from collections import namedtuple
import requests
import random
from hashlib import sha224

Path("db").mkdir(exist_ok=True)
Datum = namedtuple("Datum", ["depth", "domain", "html", "links"])

def get_digest(url):
    return sha224(bytes(url, "utf8")).hexdigest()[:24]


def flash(url, datum):
    # keyとなるurlのhash値を計算
    key = get_digest(url)
    # valueとなるDatum型のシリアライズと圧縮
    value = gzip.compress(pickle.dumps(datum))
    # dbフォルダーにhash値のファイル名で書き込む
    with Path(f"db/{key}", "wb").open() as f:
        f.write(value)


def isExists(url):
    # keyとなるurlのhash値を計算
    key = get_digest(url)
    # もし、キーとなるファイルが存在していたら、それは過去にスクレイピングしたURLであるためskip
    if Path(f"db/{key}").exists():
        return True
    else:
        return False


# ランダムなURLをスクレイピングしたとする
dummy_urls = [f"{k:04d}" for k in range(1000)]
for i in range(10000):
    dummy_url = random.choice(dummy_urls)
    # すでにスクレイピングしていたURLならスキップする
    if isExists(dummy_url) is True:
        continue
    depth = 1
    dummy_html = """
    <html>
      <body>
        <div>
          <h1> Hello World </h1>
        </div>
      </body>
    </html>
    """
    dummy_domain = "example.com"
    dummy_links = [f"{dummy_domain}/hoge", f"{dummy_domain}/fuga"]
    datum = Datum(depth=depth, domain=dummy_domain, html=dummy_html, links=dummy_links)
    flash(dummy_url, datum)
```
