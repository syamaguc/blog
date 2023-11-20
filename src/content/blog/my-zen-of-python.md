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

## Package管理

基本的な方針としては、

1. pythonのバージョン管理は`pyenv`
1. packageの管理は

- script程度なら`venv`
- しっかりとしたPJなら`poetry`

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

### Multiprocess

Processのほうが生成コストが低い。Processのほうは並列化して実行する関数ごとにオブジェクトが生成される。そのため、呼び出す関数の数が膨大だとその生成コストが高くなったりメモリを食う。
一方で、Poolのほうは基本的には引数でコア数などでworker数を指定して、各worker数ごとにタスクの処理が開始し、1つの処理が終わったらキューにある次のタスクの処理を開始する。
そのため、呼び出す関数が膨大でもProcessのように膨大にオブジェクトが作られたりはしない。

上記をふまえ、

- 呼び出す関数の数が膨大で、1つ1つのタスクがライトな場合 -> Processで大量にオブジェクトを生成するのはコストが高いのでPoolを使う。
- 呼び出す関数は少ないけど、1つ1つの関数の処理時間が長い場合 -> 単体の生成コストが低いProcessを使う

めんどくさい時はとりあえず`concurrent.futures`でなんとかなる。

```python
import concurrent.futures
import math

PRIMES = [
    112272535095293,
    112582705942171,
    112272535095293,
    115280095190773,
    115797848077099,
    1099726899285419]

def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    sqrt_n = int(math.floor(math.sqrt(n)))
    for i in range(3, sqrt_n + 1, 2):
        if n % i == 0:
            return False
    return True

def main():
    with concurrent.futures.ProcessPoolExecutor() as executor:
        for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
            print('%d is prime: %s' % (number, prime))

if __name__ == '__main__':
    main();
```

#### tor

- スクレイピング時のIPアドレス秘匿

```python
import requests
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

def setup_driver():
    """ """
    #NOTE:socks5hのほうが望ましいが、エラーなる
    PROXY = "socks5://localhost:9050"
    options = Options()
    options.add_argument("--headless")
    options.add_argument("--disable-gpu")
    options.add_argument("--disable-extensions")
    options.add_argument("--proxy-server=%s" % PROXY)
    driver = webdriver.Chrome(options=options)
    driver.set_window_size(50, 50)
    driver.implicitly_wait(30)
    driver.set_page_load_timeout(30)
    return driver


def check_tor_requests():
    proxies = {
        "http": "socks5h://localhost:9050",
        "https": "socks5h://localhost:9050",
    }
    response = requests.get("https://check.torproject.org/api/ip", proxies=proxies)
    if response.json()["IsTor"] is not True:
        return False
    else:
        print(f"Tor is running: {response.json()['IP']}")
        return True


def check_tor_selenium():
    driver = setup_driver()
    driver.get("https://check.torproject.org/")

    title = driver.title.lower()
    if "congratulations" in title:
        print(title)
        return True
    else:
        print(title)
        return False


if __name__ == "__main__":
    check_tor_requests()
    check_tor_selenium()

```

### namedtuple

- ex, 自作のファイルシシテムベースのKVS

```python
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
    key = get_digest(url)
    value = gzip.compress(pickle.dumps(datum))
    with Path(f"db/{key}", "wb").open() as f:
        f.write(value)


def isExists(url):
    key = get_digest(url)
    if Path(f"db/{key}").exists():
        return True
    else:
        return False


if __name__ == "__main__":
  dummy_urls = [f"{k:04d}" for k in range(1000)]
  for i in range(10000):
      dummy_url = random.choice(dummy_urls)
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
