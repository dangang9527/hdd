# 黑点工具 · Heidian Toolkit

> 一个完全在浏览器里运行的在线工具箱：74 个工具、5 大分类，覆盖图片、PDF、文本、音视频与开发者常用操作。文件不上传、不需要登录、不需要后端，打开网页就能用。

**A client-side-only toolbox: 74 utilities, zero uploads, zero backend, zero build step.**

![Static site](https://img.shields.io/badge/site-static-2f6f4f)

![No build](https://img.shields.io/badge/build-none-2f6f4f)

![No backend](https://img.shields.io/badge/backend-none-2f6f4f)

![Files stay local](https://img.shields.io/badge/files-stay%20local-blue)

![License: MIT](https://img.shields.io/badge/license-MIT-blue)

---

## 这是什么

黑点工具是一个**纯静态多页工具站**。所有文件处理都发生在浏览器内：Canvas 2D、WebAssembly、Web Audio、MediaRecorder、原生二进制解析，不经过任何服务器。整站由原生 HTML / CSS / JavaScript 写成，没有框架、没有打包器、没有 `npm install`，把 `dist/` 目录丢到任意静态托管上就能跑。

它同时是一个**认真做 SEO 的静态站**：117 个预渲染 HTML 页面各自拥有独立的标题、正文、FAQ、canonical 与 JSON-LD 结构化数据；运行时再接管为无刷新路由（SPA），两套模式共用同一份工具定义，不会出现「爬虫看到空壳」的问题。

### 一眼看懂

| 项目              | 数值                                            |
| --------------- | --------------------------------------------- |
| 主工具             | 74 个（图片 30 / 文本 12 / 开发者 13 / PDF 10 / 音视频 9） |
| 参数预设规格页         | 35 个（独立 URL，参数已预填，不进主列表）                      |
| 预渲染 HTML 页面     | 117 个                                         |
| sitemap 可索引 URL | 116 个                                         |
| 运行时依赖           | 无框架、无构建；8 个第三方库按需从 CDN 懒加载                    |
| 后端 / 数据库 / 账号   | 无                                             |
| 开源许可            | MIT（品牌与商标资产除外，见 License）                      |

---

## 设计原则

**1. 文件不出浏览器。** 图片、PDF、音视频、文本全部用 `File` / `Blob` / `ArrayBuffer` 在本地处理，不发起上传请求。没有服务端，也就没有可以泄露的文件副本。

**2. 零构建。** `dist/` 就是最终产物，浏览器直接执行源码。`app.js` 是原生 ES5+ 语法写成的单文件应用，改完刷新即生效，不需要编译、不需要依赖树。

**3. 一套定义，两种渲染。** 每个工具用 `tool({ ... })` 注册一次，既用于运行时路由渲染，也用于生成静态 HTML。工具页的正文、FAQ、结构化数据来自同一份 `SEO_CONTENT` 定义，静态页与 JS 路由永远一致。

**4. 按需加载，不拖慢首屏。** 体积大的库（HEIC 解码器、PDF.js、MP3 编码器等）只在用户真正打开对应工具时才从 CDN 拉取。首屏只加载本站的 CSS 与 JS。

**5. 内存是有上限的资源。** 批量处理遵循「逐张解码 → 处理 → 导出 → 立即释放」。以 HEIC 批量转换为例：单画布复用、200 px 缩略图、解码期直接缩放、默认输出长边 1920 px、400 MB 输出预算、随时可取消。这套机制来自一次真实事故的修复——20 张 12 MP 的 HEIC 照片曾让浏览器峰值涨到 1.85 GB 并崩溃，修复后同场景峰值增量降到约 392 MB。

**6. 视觉统一。** `ui-2026.css` 以设计令牌叠加在原样式之上，含浅/深主题、桌面侧栏与移动端底部导航。新增工具复用 `Field` / `DropZone` / `toolUI` / `Result` / `Progress`，无需另写样式。

---

## 功能一览

<details>

<summary><strong>图片（30）</strong></summary>

格式转换、压缩（按质量或按目标体积）、调整尺寸、裁剪、批量裁剪、批处理工作台、旋转翻转、拼接、长图切分、水印、标注与打码、证件照、Favicon 生成、占位图、字符画、Base64 互转、图片去重、图片对比、SVG 优化、SVG 转 PNG、EXIF 查看与清除、EXIF 编辑与写入、DPI 设置、HEIC 照片转换，以及新增的调色与滤镜、像素化与故障特效、圆角与边框、图片叠加混合、配色提取、颜色替换与去背景。

</details>

<details>

<summary><strong>PDF 与文档（10）</strong></summary>

PDF 压缩 / 合并 / 拆分 / 旋转 / 转图片 / 加水印 / 加页码 / 页面管理 / 元数据编辑，图片转 PDF。

</details>

<details>

<summary><strong>文本与数据（12）</strong></summary>

JSON 格式化与校验、JSON ↔ CSV、JSON → YAML、Markdown → HTML、Base64 编解码、URL 编解码、文本对比 / 清理 / 统计 / 行处理、命名与大小写转换、代码格式化与压缩。

</details>

<details>

<summary><strong>音视频（9）</strong></summary>

音频格式转换、音频剪辑、视频提取音频、视频截帧、视频转 GIF、GIF 工具箱（拆帧 / 合成 / 调速 / 裁剪）、GIF 倒放、GIF 加文字、GIF 转 MP4 / WebM。

</details>

<details>

<summary><strong>开发者（13）</strong></summary>

哈希计算、JWT 解码、二维码生成与解码、正则测试、时间戳转换、Cron 表达式解析、进制转换、单位换算、颜色格式转换、UUID / 随机 ID、TOTP 验证码、批量重命名与打包。

</details>

**35 个参数预设规格页**：压缩到 20 KB – 2 MB、调整到 64×64 – 3000×3000、PNG / WebP / AVIF 定向转换等。它们是「打开就填好参数的真实工具页」，不是引导页；全部隐藏在 `VARIANTS` 中，不进主工具列表，因此不会稀释首页卡片与分类计数。

---

## 目录结构

```
.
├── README.md                   # 本文件
├── LICENSE                     # MIT
├── THIRD_PARTY_NOTICES.md      # 第三方依赖许可与 LGPL 说明
├── .gitignore
├── .github/workflows/pages.yml # 可选：自动部署到 GitHub Pages
├── dist/                       # 站点产物，部署这个目录
│   ├── index.html              # 首页（预渲染）
│   ├── image/ media/ pdf/ text/ dev/     # 分类页与工具页
│   ├── assets/
│   │   ├── app.js              # 工具定义 + 路由 + SEO，单文件主程序
│   │   ├── style.css / ui-2026.css / ui-2026.js
│   │   └── analytics.js        # 可选统计，开源版默认关闭
│   ├── sw.js                   # Service Worker，网络优先缓存
│   └── sitemap.xml  robots.txt  llms.txt  llms-full.txt  manifest.webmanifest
├── add-preset-pages.py / add-b-tools.py   # 注入、预渲染、SEO 同步
├── b-tools.snippet.js / heic-tool.snippet.js   # 工具源码片段
├── preset-shell-template.html  # 预渲染 HTML 模板（生成脚本依赖）
└── verify-*.py                 # Playwright 回归脚本
```

> 本仓库是**可直接公开的开源版**：已移除真实统计 ID、搜索引擎站长验证文件、历史备份、预览截图与测试素材；canonical / sitemap / llms 中的域名统一替换为占位地址 `https://example.com`，部署前按文末「部署前配置」替换即可。

---

## 快速开始

不需要 Node，不需要安装依赖。任意静态服务器即可（不能直接用 `file://`，因为模块与 SW 需要 HTTP）：

```bash
cd dist
python -m http.server 8765 --bind 127.0.0.1
# 打开 http://127.0.0.1:8765/
```

也可以用 `npx serve dist`、`php -S 127.0.0.1:8765 -t dist`。部署到 Cloudflare Pages / Netlify / Vercel 时**发布目录填 `dist`、构建命令留空**；部署到 GitHub Pages 可用仓库自带的 `pages.yml`（见「部署前配置」）。

注意：

- 依赖 CDN 的工具（HEIC、PDF、Markdown、二维码、MP3 等）首次打开需要联网；离线时这些工具不可用，其余工具正常。
- 开源版**默认不加载任何第三方统计**；Service Worker 在本地 / 内网地址下自动停用，不会缓存开发中的文件。
- 直接访问 `/image/compress/` 这类深层 URL 需要托管服务支持「目录 + index.html」回退；不存在的地址必须返回真实 HTTP 404，不能统一重写为 200 首页。

---

## 架构：如何新增一个工具

所有工具都在 `dist/assets/app.js` 中用同一个 `tool()` 注册，页面、路由、导航、SEO 与结构化数据由共享层自动生成：

```js
tool({
  id: 'image-demo',
  cat: 'image',                       // image | pdf | text | media | dev
  name: '示例工具',
  ico: 'compress',
  desc: '一句话说明这个工具做什么',
  tags: ['示例', 'demo'],
  faq: [{ q: '会上传我的图片吗？', a: '不会，全部在浏览器内处理。' }],
  mount(root, ctx) {
    const dz = DropZone({ accept: 'image/*', multiple: true });
    const ratio = Field.slider({ label: '强度', min: 0, max: 100, value: 50, unit: '%' });
    const fmt = Field.select({ label: '输出格式', value: 'png', options: ['png', 'jpeg'] });
    const ui = toolUI({
      zone: dz.dz,
      opts: [ratio.el, fmt.el],
      btnLabel: '开始处理',
      // onRun 收到 { prog, res, err, btn, signal }，文件从 dz.dz.get() 取
      onRun: async ({ prog, res, signal }) => {
        const files = dz.dz.get();
        /* 逐张解码 → Canvas 处理 → 导出 Blob → res.add(...) */
      },
    });
    root.appendChild(ui.panel);
  },
});
```

工具的详细正文、使用场景与扩展 FAQ 放在同文件的 `SEO_CONTENT[id]` 表里，由 `toolDocumentation()` 合并后生成静态页正文与 JSON-LD。

常用组件：`DropZone`（拖拽与文件选择）、`Field.slider / select / number / text / toggle / seg`、`toolUI`（设置区 + 主按钮 + 进度 + 结果 + 错误）、`Result`（下载列表）、`Progress`（可取消进度条）、`previewBox`（画布预览）、`encodeGif` / `decodeGifFrames`（动图编解码）。

其中 12 个新增工具与 HEIC 工具的源码分别保存在 `b-tools.snippet.js`、`heic-tool.snippet.js`，由 `app.js` 中的 `registerBTools()` 与 HEIC 模块注册。**要修改这些工具请改片段文件，再用 `inject` 重新注入**，不要直接编辑注入后的产物，否则下次生成会被覆盖。

新增工具后需要生成对应的静态页并同步 SEO 面，仓库里的脚本把这件事做成了一条命令：

```bash
python add-b-tools.py inject      # 把工具片段注入 app.js
python add-b-tools.py render      # Playwright 预渲染全部页面
python add-b-tools.py fixall      # 按页面深度重锚定资源路径
python add-b-tools.py sync        # sitemap / llms / 计数 / SW 版本
python add-b-tools.py verify      # 静态与浏览器回归
python add-b-tools.py pack        # 打包可部署 zip
```

部署前记得更新 `sw.js` 里的缓存版本号，否则老用户会被旧缓存锁住。

---

## 测试

`verify-*.py` 是一组基于 Playwright + 本机 Chrome 的回归脚本，覆盖真实文件处理而不只是「页面没报错」：

| 脚本                                                    | 覆盖                                        |
| ----------------------------------------------------- | ----------------------------------------- |
| `verify-ui.py`                                        | 全工具页挂载、26 组屏宽、主题与导航交互                     |
| `verify-seo-analytics.py`                             | canonical / JSON-LD / sitemap 一致性 / PV 上报 |
| `verify-heic.py`、`verify-heic-stress.py`              | HEIC 单张与批量、内存峰值、异常文件                      |
| `verify-preset-function.py`、`verify-preset-visual.py` | 规格页预设生效、真实图片端到端、响应式                       |
| `verify-b-tools.py`、`verify-b-visual.py`              | 新增工具输出文件内容校验、48 组响应式                      |

脚本默认使用 Playwright 自带的 Chromium（`pip install playwright && playwright install chromium`）；要用本机 Chrome 时设置环境变量 `CHROME_PATH` 指向可执行文件。测试站点地址默认 `http://127.0.0.1:8765`，可用 `SITE_BASE` 覆盖。`verify-seo-analytics.py` 会注入一个测试用 ID 来验证 PV 队列逻辑，所有外部请求都在本地 mock，不连接任何统计服务；线上启用统计仍需按文末方式自行配置。

当前基线：本机 Chrome 实测全部通过，零页面错误；像素、色彩、EXIF 字节、DPI、动图帧序与导出 MIME 都做了断言。

涉及真实文件的脚本（`verify-heic*.py`、`verify-b-tools.py`、`verify-preset-function.py`）需要自备测试图片：在仓库根创建 `.test-fixtures/` 并放入对应文件。该目录不随仓库分发（已在 `.gitignore` 中），缺少素材时脚本会给出明确提示。`verify-ui.py` 里关于内部设计展示页的检查在该文件不存在时自动跳过。

---

## 隐私说明

本站不上传文件内容，也没有账号体系。但仍需如实说明三点：

1. **统计默认关闭**：开源版的 `dist/assets/analytics.js` 不含任何统计 ID，未配置时直接返回，不发起第三方请求。若自行启用，会上报页面路径与标准浏览器 / 来源信息，但不会发送文件内容、表单输入、查询串或锚点；本地与内网环境始终跳过。
2. **CDN 请求**：首次打开特定工具时会向 jsDelivr / cdnjs 拉取库文件，CDN 方能观察到请求 IP。若要求完全自托管，把这些库下载到 `dist/vendor/` 并改写引用即可。
3. **浏览器能力**：GIF 转视频用 `MediaRecorder` 实时录制，耗时约等于动图时长，且需保持页面在前台；EXIF 写入会重建 APP1 段，原 GPS 等字段会被清除（页面内有警示）。

---

## 第三方依赖与许可

以下库按工具需要**动态从 CDN 加载**，仓库本身不包含其二进制或源码副本：

| 库                  | 版本       | 用途                                         | 上游许可（请自行复核）  |
| ------------------ | -------- | ------------------------------------------ | ------------ |
| `heic-to`          | 1.6.7    | HEIC / HEIF 解码（内含 libheif + libde265 WASM） | **LGPL-3.0** |
| `pdfjs-dist`       | 3.11.174 | PDF 渲染                                     | Apache-2.0   |
| `pdf-lib`          | 1.17.1   | PDF 生成与编辑                                  | MIT          |
| `lamejs`           | 1.2.1    | MP3 编码                                     | **LGPL-3.0** |
| `jsqr`             | 1.4.0    | 二维码识别                                      | Apache-2.0   |
| `qrcode-generator` | 1.4.4    | 二维码生成                                      | MIT          |
| `js-yaml`          | 4.1.0    | YAML 转换                                    | MIT          |
| `marked`           | 12.0.2   | Markdown 渲染                                | MIT          |

**关于 LGPL**：`heic-to` 与 `lamejs` 涉及 LGPL-3.0。当前做法是按 URL 运行时加载、未打包进仓库，通常不构成「随本仓库再分发该库」；但如果你把库文件 vendored 进仓库、或分发离线包，就需要遵守 LGPL 的可替换性要求（保留上游源码获取途径、允许使用者替换该库），并提供相应的许可文本与版权声明。请逐项核对各上游仓库的 LICENSE 与 NOTICE，完整声明见 [THIRD\_PARTY\_NOTICES.md](THIRD_PARTY_NOTICES.md)。

本站所有工具文案、FAQ、图标与 UI 排版均为原创，未复制任何第三方站点的内容、源码或模型文件。

---

## 部署前配置

这份仓库已经完成公开脱敏（真实统计 ID、站长验证文件、备份与测试素材均已移除），  
但部署到你自己的域名前仍需处理：

- [ ] **域名**：全站 canonical、JSON-LD、og:image、`sitemap.xml`、`robots.txt`、`llms*.txt` 目前统一使用占位地址 `https://example.com`（约 2400 处）。全局替换成你的域名，同步修改 `dist/assets/app.js` 的 `SEO_ORIGIN` 与两个生成脚本的 `ORIGIN`，然后重跑 `render / fixall / sync`。部署到子目录还要重算资源相对路径与 `sw.js` 的 `CORE` 列表。
- [ ] **统计（可选）**：需要 PV 时，设置 `dist/assets/analytics.js` 的 `BAIDU_HM_ID`，或在该脚本加载前设置 `window.HEIDIAN_ANALYTICS_ID`。**不要把生产统计 ID 提交到公开仓库**。
- [ ] **版权署名**：`LICENSE` 与 `THIRD_PARTY_NOTICES.md` 的版权行目前是通用写法（`Heidian Toolkit contributors`），换成你自己的名字或组织名。
- [ ] **品牌资产**：`dist/assets/logo.svg`、`og.png`、favicon 与站点名称不在 MIT 代码许可范围内，如非自有请替换。
- [ ] **GitHub Pages（可选）**：仓库自带 `.github/workflows/pages.yml`。在 Settings → Pages → Source 选择 "GitHub Actions"，之后每次 push 到 main 会自动部署 `dist/`。Pages 通常是子路径 URL，务必先完成上面的域名与相对路径替换，否则 canonical 和资源路径会错。
- [ ] **发布顺序**：新增 URL 建议分批向搜索平台提交，避免一次性铺开被判低质量页面。

---
