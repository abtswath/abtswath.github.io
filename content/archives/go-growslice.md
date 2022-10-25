---
title: "Go 切片扩容"
date: 2022-10-21T17:33:32+08:00
draft: false
categories: 笔记
tags: 
    Go
---

### 1.18 之前

1. 当前容量小于 1024 时，新容量为当前容量的两倍；
2. 当前容量大于等于 1024 时，新容量为当前容量的 1.25 倍。

### 1.18之后

1. 当前容量小于 256 时，新容量为当前容量两倍；
2. 当前容量大于等于 256 时，新容量计算公式为 当前容量 + (当前容量 + 3 * 256) / 4。

> [https://github.com/golang/go/commit/2dda92ff6f9f07eeb110ecbf0fc2d7a0ddd27f9d](https://github.com/golang/go/commit/2dda92ff6f9f07eeb110ecbf0fc2d7a0ddd27f9d)
> [https://groups.google.com/g/golang-nuts/c/UaVlMQ8Nz3o](https://groups.google.com/g/golang-nuts/c/UaVlMQ8Nz3o)
