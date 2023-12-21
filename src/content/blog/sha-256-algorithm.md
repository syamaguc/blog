---
title: Sha256 Algorithm
author: syamaguc
pubDatetime: 2023-05-11T00:00:00Z
postSlug: sha-256-alogorithm
featured: false
draft: false
tags:
  - dev
  - cryptography
description: ビットコインを守る最後の砦、Sha256
---

## Table of Contents

## sha256とは

### ✅ SHA-256の特性

1. 固定長出力: どのような長さの入力データであっても、出力は常に256ビット（32バイト）
1. 衝突耐性: 同じハッシュ値を持つ2つのなる入力データを見つけるのは非常に困難である。
1. 事前像耐性: 与えられたハッシュ値から、元のデータを再構築するのは非常に困難である。
1. 2次事前像耐性: 与えられたデータから、同じハッシュ値を持つ別のデータを見つけるのは非常に困難である。

ビットコインやSSH, TLS/SSLなど現代のテクノロジーを支える重要な暗号学的ハッシュ関数。
アメリカ国家安全保障局によって設計され、2001年にアメリカ国立標準技術研究所によって標準化された。
ソースコードはオープンソースで公開されている。

## 処理の概要

### 入力をbit変換(0 or 1)

- ex, `unko puripuri`

```
# bit

01110101 01101110 01101011 01101111
00100000 01110000 01110101 01110010
01101001 01110000 01110101 01110010
01101001

```

### message block(32bits × 16行 = 512 bits)の生成

1. 元のメッセージ長（この場合104 -> 10進数で`1101000`）をメッセージブロックの最後に追加する。
1. メッセージブロックが512の倍数になるよう、ゼロでパディングする。(104 + 1 + 343 + 64 = 512)

```
# Message block - 512 Bits

01110101 01101110 01101011 01101111
00100000 01110000 01110101 01110010
01101001 01110000 01110101 01110010
01101001 10000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 00000000
00000000 00000000 00000000 01101000

```

### message schedule(32bits × 64行 = 2048bits)の生成

1. w[0..15]にメッセージブロックをコピーする
1. 各message blockのn行目をw[n]として、以下の作業をn = 0 から n < 48までループ

- σ0 = (w[n+1] right rotate 7) xor (w[n+1] right rotate 18) xor (w[n+1] right shift 3)
- σ1 = (w[n+14] right rotate 17) xor (w[n+14] right rotate 19) xor (w[n+14] right shift 10)
- w[n+16]= w(n) + σ0 + w[n+9] + σ1

```
Message schedule - 1st chunk
w0  01110101011011100110101101101111
w1  00100000011100000111010101110010
w2  01101001011100000111010101110010
w3  01101001100000000000000000000000
w4  00000000000000000000000000000000
.
.
.
w15 00000000000000000000000001101000
w16 00000000000000000000000000000000
.
.
.
w63 00000000000000000000000000000000

```

### hashの計算

- 初期Hash

  - 2, 3, 5, 7, 11, 13, 17, 19の立方根の小数部分最初の8桁から派生

    - h0 = 6a09e667 = a
    - h1 = bb67ae85 = b
    - h2 = 3c6ef372 = c
    - h3 = a54ff53a = d
    - h4 = 510e527f = e
    - h5 = 9b05688c = f
    - h6 = 1f83d9ab = g
    - h7 = 5be0cd19 = h

- K定数(k[64] = [0x428a2f98,0x71374491, ... ,0xbef9a3f7,0xc67178f2])をセットする

  - 最初の64個の素数の小数部の最初の32bit
  - 恐らく、定数に明確な素数を採用することで、人為的バックドアの懸念を晴らしたかった？

- 各message scheduleのn行目をw[n]として、以下の作業をn = 0 -> n < 64までループ
  - Σ0 = (a right rotate 2) xor (a right rotate 13) xor (a right rotate 22)
  - Σ1 = (e right rotate 6) xor (e right rotate 11) xor (e right rotate 25)
  - Choice = (e and f) xor ((not e) and g)
  - Majority = (a and b) xor (a and c) xor (b and c)
  - Temp1 = h + Σ1 + Choice + k[n] + w[n]
  - Temp2 = Σ0 + Majority
  - 値をswap
    - h = g
    - g = f
    - f = e
    - e = d + Temp1
    - d = c
    - c = b
    - b = a
    - a = Temp1 + Temp2

### 仕上げ

a, b, c, d, e, f, g, hの値をそれぞれ16進数変換して連結する

## Cでの実装例

```c
#include "stdio.h"
#include "stdlib.h"
#include "string.h"

#define uchar unsigned char
#define uint unsigned int

#define DBL_INT_ADD(a,b,c) if (a > 0xffffffff - (c)) ++b; a += c;
#define ROTLEFT(a,b) (((a) << (b)) | ((a) >> (32-(b))))
#define ROTRIGHT(a,b) (((a) >> (b)) | ((a) << (32-(b))))

#define CH(x,y,z) (((x) & (y)) ^ (~(x) & (z)))
#define MAJ(x,y,z) (((x) & (y)) ^ ((x) & (z)) ^ ((y) & (z)))
#define EP0(x) (ROTRIGHT(x,2) ^ ROTRIGHT(x,13) ^ ROTRIGHT(x,22))
#define EP1(x) (ROTRIGHT(x,6) ^ ROTRIGHT(x,11) ^ ROTRIGHT(x,25))
#define SIG0(x) (ROTRIGHT(x,7) ^ ROTRIGHT(x,18) ^ ((x) >> 3))
#define SIG1(x) (ROTRIGHT(x,17) ^ ROTRIGHT(x,19) ^ ((x) >> 10))

typedef struct {
uchar data[64];
uint datalen;
uint bitlen[2];
uint state[8];
} SHA256_CTX;

/* K constants */
uint k[64] = {
0x428a2f98,0x71374491,0xb5c0fbcf,0xe9b5dba5,0x3956c25b,0x59f111f1,0x923f82a4,0xab1c5ed5,
0xd807aa98,0x12835b01,0x243185be,0x550c7dc3,0x72be5d74,0x80deb1fe,0x9bdc06a7,0xc19bf174,
0xe49b69c1,0xefbe4786,0x0fc19dc6,0x240ca1cc,0x2de92c6f,0x4a7484aa,0x5cb0a9dc,0x76f988da,
0x983e5152,0xa831c66d,0xb00327c8,0xbf597fc7,0xc6e00bf3,0xd5a79147,0x06ca6351,0x14292967,
0x27b70a85,0x2e1b2138,0x4d2c6dfc,0x53380d13,0x650a7354,0x766a0abb,0x81c2c92e,0x92722c85,
0xa2bfe8a1,0xa81a664b,0xc24b8b70,0xc76c51a3,0xd192e819,0xd6990624,0xf40e3585,0x106aa070,
0x19a4c116,0x1e376c08,0x2748774c,0x34b0bcb5,0x391c0cb3,0x4ed8aa4a,0x5b9cca4f,0x682e6ff3,
0x748f82ee,0x78a5636f,0x84c87814,0x8cc70208,0x90befffa,0xa4506ceb,0xbef9a3f7,0xc67178f2
};

void SHA256Transform(SHA256_CTX *ctx, uchar data[])
{
uint a, b, c, d, e, f, g, h, i, j, t1, t2, m[64];

    for (i = 0, j = 0; i < 16; ++i, j += 4)
    	m[i] = (data[j] << 24) | (data[j + 1] << 16) | (data[j + 2] << 8) | (data[j + 3]);
    for (; i < 64; ++i)
    	m[i] = SIG1(m[i - 2]) + m[i - 7] + SIG0(m[i - 15]) + m[i - 16];

    a = ctx->state[0];
    b = ctx->state[1];
    c = ctx->state[2];
    d = ctx->state[3];
    e = ctx->state[4];
    f = ctx->state[5];
    g = ctx->state[6];
    h = ctx->state[7];

    for (i = 0; i < 64; ++i) {
    	t1 = h + EP1(e) + CH(e, f, g) + k[i] + m[i];
    	t2 = EP0(a) + MAJ(a, b, c);
    	h = g;
    	g = f;
    	f = e;
    	e = d + t1;
    	d = c;
    	c = b;
    	b = a;
    	a = t1 + t2;
    }

    ctx->state[0] += a;
    ctx->state[1] += b;
    ctx->state[2] += c;
    ctx->state[3] += d;
    ctx->state[4] += e;
    ctx->state[5] += f;
    ctx->state[6] += g;
    ctx->state[7] += h;

}

void SHA256Init(SHA256_CTX *ctx)
{
ctx->datalen = 0;
ctx->bitlen[0] = 0;
ctx->bitlen[1] = 0;
/*最初の8つの素数（2, 3, 5, 7, 11, 13, 17, 19）の平方根の小数部の最初の32ビット*/
ctx->state[0] = 0x6a09e667;
ctx->state[1] = 0xbb67ae85;
ctx->state[2] = 0x3c6ef372;
ctx->state[3] = 0xa54ff53a;
ctx->state[4] = 0x510e527f;
ctx->state[5] = 0x9b05688c;
ctx->state[6] = 0x1f83d9ab;
ctx->state[7] = 0x5be0cd19;
}

void SHA256Update(SHA256_CTX *ctx, uchar data[], uint len)
{
for (uint i = 0; i < len; ++i) {
ctx->data[ctx->datalen] = data[i];
ctx->datalen++;
if (ctx->datalen == 64) {
SHA256Transform(ctx, ctx->data);
DBL_INT_ADD(ctx->bitlen[0], ctx->bitlen[1], 512);
ctx->datalen = 0;
}
}
}

void SHA256Final(SHA256_CTX *ctx, uchar hash[])
{
uint i = ctx->datalen;

    if (ctx->datalen < 56) {
    	ctx->data[i++] = 0x80;
    	while (i < 56)
    		ctx->data[i++] = 0x00;
    }
    else {
    	ctx->data[i++] = 0x80;
    	while (i < 64)
    		ctx->data[i++] = 0x00;
    	SHA256Transform(ctx, ctx->data);
    	memset(ctx->data, 0, 56);
    }

    DBL_INT_ADD(ctx->bitlen[0], ctx->bitlen[1], ctx->datalen * 8);
    ctx->data[63] = ctx->bitlen[0];
    ctx->data[62] = ctx->bitlen[0] >> 8;
    ctx->data[61] = ctx->bitlen[0] >> 16;
    ctx->data[60] = ctx->bitlen[0] >> 24;
    ctx->data[59] = ctx->bitlen[1];
    ctx->data[58] = ctx->bitlen[1] >> 8;
    ctx->data[57] = ctx->bitlen[1] >> 16;
    ctx->data[56] = ctx->bitlen[1] >> 24;
    SHA256Transform(ctx, ctx->data);

    for (i = 0; i < 4; ++i) {
    	hash[i] = (ctx->state[0] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 4] = (ctx->state[1] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 8] = (ctx->state[2] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 12] = (ctx->state[3] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 16] = (ctx->state[4] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 20] = (ctx->state[5] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 24] = (ctx->state[6] >> (24 - i * 8)) & 0x000000ff;
    	hash[i + 28] = (ctx->state[7] >> (24 - i * 8)) & 0x000000ff;
    }

}

char* SHA256(char* data) {
int strLen = strlen(data);
SHA256_CTX ctx;
unsigned char hash[32];
char *hashStr = malloc(65);
strcpy(hashStr, "");

    SHA256Init(&ctx);
    SHA256Update(&ctx, data, strLen);
    SHA256Final(&ctx, hash);

    char s[3];
    for (int i = 0; i < 32; i++) {
    	sprintf(s, "%02x", hash[i]);
    	strcat(hashStr, s);
    }

    return hashStr;

}

```

### Example

```c
int main(void){
  char data[] = "unko puripuri";
  char* sha256 = SHA256(data);
  printf("%s\n", sha256);
}

```

### Output

```
1dbf03acce51637e1a7619ec87668ff718d7b53cba8d58225dbfca825c4d7f16

```
