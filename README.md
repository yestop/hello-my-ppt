# hello-my-ppt

本地 PPTD → 可编辑 PPTX 技能。它把 PPTD 项目、浏览器编辑、PPTX 导出和页面图片渲染全部放在本地运行，不依赖 Kimi 登录、Kimi 下载接口或远程 PPTX 转换服务。

## 能做什么

- 创建和编辑 PPTD v2 项目：`deck.pptd` + `pages/` + `media/`
- 在 Chrome/Edge 中本地预览、实时刷新和保存 PPTD
- 用本地 OOXML writer 生成可编辑 `.pptx`
- 用同一套本地渲染管线生成 PNG 页面图片
- 导出完整项目 ZIP
- 支持文本、形状、表格、11 类图表、公式、SVG/PNG/JPEG/GIF、图标和可选字体嵌入

## 快速开始

要求 Node.js 18+；不需要 `npm install`。

```powershell
# 进入 skill 目录
cd C:\path\to\hello-my-ppt

# 启动本地编辑器
node bin/hello-my-ppt.js serve --project C:\path\to\my-deck

# 本地生成 PPTX，不经过 Kimi
node bin/hello-my-ppt.js export C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\deck.pptx

# 生成页面图片
node bin/hello-my-ppt.js render C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\rendered

# 打包完整 PPTD 项目
node bin/hello-my-ppt.js export-project C:\path\to\my-deck\deck.pptd `
  -o C:\path\to\my-deck\deck-project.zip
```

编辑器启动后会打印本地地址，例如 `http://127.0.0.1:55173/`。用 Chrome 或 Edge 打开即可。`serve` 用于编辑和预览，`export` 用于生成 PPTX，`render` 用于生成图片。

## 推荐项目结构

```text
my-deck/
├── deck.pptd
├── pages/
│   ├── 01.page
│   └── 02.page
└── media/
    └── robot.png
```

所有页面和媒体路径都必须是相对于 `deck.pptd` 的项目内路径。

## 校验 PPTX

```powershell
node tests/package-integrity.mjs C:\path\to\my-deck\deck.pptx 24
```

高风险交付仍应使用 PowerPoint/WPS 实际打开并检查代表性页面。

## 限制

- 不直接把现有 PPTX 反向转换成 PPTD；现有 PPTX 需要作为参考重新构建。
- heatmap 和 Sankey 不导出为原生 PowerPoint 图表。
- 未知图标会被跳过；优先使用 `references/icons.md` 中列出的 `bs:<name>`，或将图标作为本地 SVG/PNG 图片。
- 不保留原始 PPTX 的母版结构；视觉系统应使用 PPTD theme 和原生页面元素重建。

## 许可证与来源

本发行版基于 `open-pptd` 的本地 PPTD/PPTX writer 和编辑器架构，并在此基础上增加 `hello-my-ppt` 命名、本地化说明和兼容图标映射。请同时阅读仓库中的 `NOTICE.md`。
