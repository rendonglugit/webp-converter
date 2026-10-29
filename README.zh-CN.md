# WebP 批量转换

把**整个文件夹**或 **ZIP 包**里的图片一次性转成 WebP —— 质量可调、支持真无损、导出时保留原有目录结构。

所有处理都在浏览器本地完成，**图片不会上传**，页面也不产生任何网络请求。

[English](README.md) | **简体中文**

## 在线使用

https://rendonglugit.github.io/webp-converter/

## 为什么做这个

Android Studio 里有个很好用的「Converting Images to WebP」对话框，但它只服务于 Android 工程。设计师、前端、运营手里的图，往往还是得一张张丢进在线转换网站，而那些网站通常要求你把素材传到别人的服务器上。

这个工具就是那个对话框的浏览器版：拖进去、选个质量、下载 ZIP。不用装东西，不用上传。

## 功能

| | |
|---|---|
| **输入** | 拖入文件夹、拖入 ZIP，或手动选择文件 |
| **输出** | 单个 `.webp` 文件，或打包成一个 ZIP |
| **有损编码** | 质量 0–100 可调，默认 75 |
| **无损编码** | 真无损（libwebp VP8L），输出像素完全一致 |
| **逐张审查** | 用拖拽滑块对比原图和 WebP，逐张决定保留哪个 |
| **跳过策略** | 转出来更大就保留原图；跳过九宫格 `.9.png`；跳过含透明通道的图 |
| **目录结构** | ZIP 内路径原样保留 —— 解压后可直接替换进工程 |
| **非图片文件** | `.xml` / `.json` / `.txt` 等原样打包，不破坏工程结构 |
| **格式支持** | PNG · JPG · JPEG · BMP · GIF（仅首帧）· AVIF · WebP |

## 怎么用

1. 把文件夹或 ZIP 拖到页面上（或点「选择文件夹」）
2. 在左侧调整质量，默认 75 通常就够
3. 看列表里的体积变化；想逐张把关就勾上「保存前逐张预览 / 审查」
4. 点「打包下载 ZIP」

下载的 ZIP 里，`assets/icons/a.png` 会变成 `assets/icons/a.webp`，其他文件保持原路径不动。

## 离线使用

整个工具就是**一个 HTML 文件**，WebP 编码器（libwebp 编译成的 WebAssembly）已经内嵌在里面。

把 `index.html` 存到本地，双击打开即可，断网也能用。发给同事也只需要发这一个文件。

## 浏览器支持

近几年的 Chrome、Edge、Safari、Firefox 都可以。

- 浏览器自带 WebP 编码能力时，优先用原生实现（更快）
- 不支持时自动切换到内置的 libwebp 引擎
- 右上角的徽章始终显示当前实际在用哪个引擎

## 实现说明

- **零依赖** —— 没有 npm 包、没有 CDN、没有构建步骤
- **WebP 编码** —— 能用 Canvas `toBlob('image/webp')` 就用；无损模式或原生不支持时，切到内嵌的 libwebp WASM
- **ZIP 读取** —— 手写中央目录解析 + 原生 `DecompressionStream('deflate-raw')`，同时兼容 UTF-8 和 GBK 两种文件名编码
- **ZIP 写出** —— 手写 STORE 模式打包（WebP 本身已是压缩数据，二次压缩只增加开销）
- **体积** —— 约 470 KB，其中 375 KB 是 base64 编码的 WebAssembly 编码器

## 许可

- 本工具代码：MIT，见 [LICENSE](LICENSE)
- 内嵌的 WebP 编码器：libwebp（BSD-3-Clause，Copyright (c) 2010 Google Inc.），构建产物来自 [@jsquash/webp](https://github.com/jamsinclair/jSquash)（Apache-2.0）

二次分发时请保留 [NOTICE](NOTICE) 中的版权声明。
