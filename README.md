# hello-my-ppt

本地 PPTD → 可编辑 PPTX 技能。它把 PPTD 项目、浏览器编辑、PPTX 导出和页面图片渲染全部放在本地运行。

## 能做什么

- 创建和编辑 PPTD v2 项目：`deck.pptd` + `pages/` + `media/`
- 在 Chrome/Edge 中本地预览、实时刷新和保存 PPTD
- 用本地 OOXML writer 生成可编辑 `.pptx`
- 用同一套本地渲染管线生成 PNG 页面图片
- 导出完整项目 ZIP
- 支持文本、形状、表格、11 类图表、公式、SVG/PNG/JPEG/GIF、图标和可选字体嵌入

## 设计方式

hello-my-ppt 支持三种设计入口：

1. **按提示词设计**：在 Codex 中说明主题、受众、页数、叙事结构、色板、字体、模板偏好和交付物，Codex 负责生成或修改 PPTD；CLI 负责本地编辑、导出和渲染。
2. **按内置示例设计**：复制 `examples/` 中的完整 PPTD 项目，替换内容、页面和素材。
3. **按外部 PPTX 参考重建**：把外部 PPTX 当作视觉参考，重建为 PPTD；hello-my-ppt 不直接导入并保留原始 PPTX 母版结构。

示例提示词：

```text
使用 hello-my-ppt 设计一份 8 页的具身智能芯片战略汇报 PPT，
面向投资人，采用高级红蓝配色，包含行业背景、技术架构、芯片路线、
竞争格局、商业模式和总结页，输出 PPTD、可编辑 PPTX 和 PNG 页面图片。
```

## 内置示例模板

这些目录是可以直接复制的 PPTD 项目，不是只能查看的截图模板：

| 目录 | 适用场景 |
|---|---|
| `business-review-7p` | 年度经营复盘、管理汇报 |
| `miaopai-saas-bp` | 商业计划、融资 BP |
| `ev-range` | 学术答辩、模型与实验结果 |
| `islelight-brand-book` | 品牌手册、视觉规范 |
| `tech-architecture-review-7p` | 技术评审、系统架构 |
| `shanmingji-2026-launch` | 产品发布、品牌发布 |
| `brand-mori-showcase-7p` | 品牌提案、创意展示 |
| `hello-my-ppt-introduction` | hello-my-ppt 使用介绍示例 |

每个示例通常包含 `deck.pptd`、`pages/`、`media/` 和可选的 `meta.yaml`。推荐先复制项目，再用 Codex 或浏览器编辑。

完整使用介绍示例：[examples/hello-my-ppt-introduction/deck.pptd](examples/hello-my-ppt-introduction/deck.pptd)。

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
- 外部 `.pptx` 模板可以用于视觉风格参考，但不会直接保留其 Slide Master、母版占位符和原始版式结构；需要直接填充原始 PPTX 母版时，请使用 `ppt-master` 或 `edit-powerpoint-live`。
- heatmap 和 Sankey 不导出为原生 PowerPoint 图表。
- 未知图标会被跳过；优先使用 `references/icons.md` 中列出的 `bs:<name>`，或将图标作为本地 SVG/PNG 图片。
- 不保留原始 PPTX 的母版结构；视觉系统应使用 PPTD theme 和原生页面元素重建。

## 许可证与来源

本发行版基于 `open-pptd` 的本地 PPTD/PPTX writer 和编辑器架构，并在此基础上增加 `hello-my-ppt` 命名、本地化说明和兼容图标映射。请同时阅读仓库中的 `NOTICE.md`。
