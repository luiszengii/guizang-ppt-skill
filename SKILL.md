---
name: guizang-tutorial-skill
description: 把一个知识点/技术概念/分析报告做成漂亮的单文件 HTML 教程或学习笔记。基于歸藏 guizang-ppt-skill 的视觉系统改造：保留 5 杂志风主题 + 4 瑞士风锚点色（共 9 种风格），但产出形态从「横向翻页 PPT」改成「纵向滚动长文档」。9 种风格**强制随机**，信息密度对齐 `~/Desktop/学习笔记/` 基线（不为视觉留白删信息）。输出归档到 `~/Desktop/学习笔记/<主题>-<要点>.html`。当用户说"做一份教程 / 学习笔记 / 知识点报告 / 把 XX 用 HTML 讲清楚"时使用。
---

# Guizang Tutorial Skill

> Fork 自 [op7418/guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)。本 skill 把原 skill 的视觉品味（5 杂志风主题 + 4 瑞士风锚点色 + Lucide 图标 + Motion 入场动画 + WebGL 背景）**完整保留**，但产出形态从「横向翻页 PPT」变成「单页纵向滚动的教程/报告/学习笔记」，并强制随机选风格、强制信息密度对齐用户既有归档基线。

## 这个 Skill 做什么

生成一份**单文件 HTML 的教程/报告/学习笔记**：

- 纵向滚动连续阅读，**不再分页**
- 9 种视觉风格**强制 bash 随机选**（5 杂志风主题 + 4 瑞士风锚点色）
- 信息密度对齐用户桌面学习笔记基线 `~/Desktop/学习笔记/`
- 输出到 `~/Desktop/学习笔记/<主题>-<要点>.html`

## 何时使用

✅ **合适**：把一个技术概念、原理、分析报告、知识点做成漂亮的 HTML 长文档归档（"用 HTML 讲一下 X"、"把这个原理做成学习笔记"、"给我做个 X 的解释报告"）

❌ **不合适**：分享 PPT / 横向翻页 deck / 演讲幻灯片 → 用原 [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)

## 工作流（4 步）

### Step 1 · 风格随机选（必做，第一步）

⚠️ **不允许 Claude 自选风格，也不要问用户选哪个。** 用 bash 跑一次随机：

```bash
awk 'BEGIN{srand();print int(rand()*9)+1}'
```

把输出（1-9）映射到下表，这次的风格就这么定了：

| 输出 | 风格 + 主题 | 模板文件 | 主题配置 |
|---|---|---|---|
| 1 | A · 🖋 墨水经典 | `assets/tutorial-template.html` | `references/themes.md` 「墨水经典」 |
| 2 | A · 🌊 靛蓝瓷 | `assets/tutorial-template.html` | `references/themes.md` 「靛蓝瓷」 |
| 3 | A · 🌿 森林墨 | `assets/tutorial-template.html` | `references/themes.md` 「森林墨」 |
| 4 | A · 🍂 牛皮纸 | `assets/tutorial-template.html` | `references/themes.md` 「牛皮纸」 |
| 5 | A · 🌙 沙丘 | `assets/tutorial-template.html` | `references/themes.md` 「沙丘」 |
| 6 | B · 🔵 克莱因蓝 IKB | `assets/tutorial-template-swiss.html` | `references/themes-swiss.md` 「IKB」 |
| 7 | B · 🟡 柠檬黄 | `assets/tutorial-template-swiss.html` | `references/themes-swiss.md` 「柠檬黄」 |
| 8 | B · 🟢 柠檬绿 | `assets/tutorial-template-swiss.html` | `references/themes-swiss.md` 「柠檬绿」 |
| 9 | B · 🟠 安全橙 | `assets/tutorial-template-swiss.html` | `references/themes-swiss.md` 「安全橙」 |

把"这次抽到 #N · 风格名"告知用户后再继续 Step 2。这是设计意图——用户喜欢这 9 种全部，所以不挑，每次开盲盒。

### Step 2 · 需求澄清（3 问）

教程是给用户反复阅读的归档，**不需要原 PPT skill 的 7 问**（受众/时长/分享场景这些无关）。只问 3 个：

| # | 问题 | 用途 |
|---|---|---|
| 1 | **主题是什么？要讲清楚的是哪个具体概念？** | 决定标题 + 章节大纲 |
| 2 | **有没有现成素材？**（文章、链接、聊天记录、上一份草稿、截图） | 有就基于素材；没有就基于 LLM 知识自由发挥但要在产出里注明信息来源置信度 |
| 3 | **目标读者熟悉度？**（小白 / 同行 / 自己复习） | 决定**深度**；但**绝不为"熟悉读者"砍信息**——本 skill 的核心是信息密度 |

**不要问风格**——Step 1 已经随机决定。如果用户主动指定风格，礼貌拒绝：「本 skill 设计上 9 种风格随机选，是核心机制；如果一定要指定，请用原 guizang-ppt-skill。」

### Step 3 · 写章节内容（信息密度是硬约束）

#### 3.1 · 信息密度参考样本（必读）

打开用户桌面已有的 `~/Desktop/学习笔记/vercel-preview-原理.html`（或 `ls ~/Desktop/学习笔记/*.html | head -3` 找最近几个看），照那个密度写。

**硬指标**：

- 总字数 ≥ 1500 字（不含 HTML 标签和注释）
- 章节数 ≥ 5 个内容章节（不算 hero/开场 + 结尾）
- 每节正文 ≥ 100 字（hero 章节例外）
- 内联 SVG 图 + 数据表合计 ≥ 2 个
- **绝不允许**「单段一句话 + 大留白」的 PPT 病——这是教程不是发布会
- **绝不允许**为了视觉留白删信息——如果某个版面塞不下，换更密的版式而不是删句子

自检命令（生成完跑一次）：

```bash
# 字数（HTML 标签也算字符所以会偏高，正文 ≥ 1500 对应总字符 ≥ 4000 左右）
wc -m ~/Desktop/学习笔记/<文件>.html
# 章节数
grep -c '<section class="slide' ~/Desktop/学习笔记/<文件>.html
# 内联 SVG 数
grep -c '<svg' ~/Desktop/学习笔记/<文件>.html
```

#### 3.2 · 操作步骤

1. **拷贝模板** 到产出位置（命名遵循 `<主题>-<要点>.html` 中文连字符风格）：

   ```bash
   mkdir -p ~/Desktop/学习笔记

   # 风格 A 教程模板（编号 1-5）
   cp <SKILL_ROOT>/assets/tutorial-template.html ~/Desktop/学习笔记/<主题>-<要点>.html

   # 风格 B 教程模板（编号 6-9）
   cp <SKILL_ROOT>/assets/tutorial-template-swiss.html ~/Desktop/学习笔记/<主题>-<要点>.html
   ```

2. **改 `<title>`** 替换 "[必填]" 占位符为真实标题

3. **应用主题色**：从对应 themes 文件复制 `:root{...}` 那几行变量（`--ink/--paper/--accent` 等）替换模板开头同名块

4. **章节用 layout 类作为骨架**——这是关键转译：
   - layouts.md（杂志风 10 种）/ layouts-swiss.md（瑞士风 22 种）里的版式骨架，在**教程模式下用作章节内排版**，不是分页
   - hero 章节：用 Layout 1（开场封面）或 S01（Cover）作为整篇的开场
   - 正文章节：穿插使用 Layout 4/5/6/9/10（杂志风）或 S03/S04/S05/S08/S11/S15-S22（瑞士风）的版式
   - 结尾章节：Layout 7/8（杂志风）或 S10/S12（瑞士风）
   - **不要每节都用同样的 layout**——节奏感来自版式变化

5. **数据/对比/流程**：能用表格的用表格，能用 SVG 流程图的用 SVG（参考 `~/Desktop/学习笔记/vercel-preview-原理.html` 那种手画风），别全堆文字

### Step 4 · 自检 + 交付

打开浏览器看（macOS）：

```bash
open ~/Desktop/学习笔记/<主题>-<要点>.html
```

对照 `references/checklist.md` 的 **「教程模式信息密度自检」** section（P0 必过）+ 原 skill 的视觉规则（继续适用，**风格 B 仍需遵守 swiss-layout-lock**）。

把产出文件路径报给用户，让他验证。

## 核心原则

1. **品味照搬，形态重构** — 保留 9 种视觉风格的所有美学细节（字体/配色/layout/Motion/WebGL），只改"分页"为"纵向流"
2. **随机优于选择** — 用户喜欢这 9 种风格全部，所以不挑，bash 抽。每次新主题像开盲盒
3. **信息密度 > 视觉留白** — 教程不是 PPT。留白美但不能牺牲内容
4. **归档优于分享** — 产出存 `~/Desktop/学习笔记/`，按 `<主题>-<要点>.html` 命名，对齐用户既有约定
5. **不带 PPT 模式** — 本 skill 专门做教程；要 PPT 用原 [guizang-ppt-skill](https://github.com/op7418/guizang-ppt-skill)

## 资源文件导览

```
guizang-tutorial-skill/
├── SKILL.md                          ← 你正在读
├── README.md                         ← Fork 缘起 + 与原 skill 区别
├── assets/
│   ├── tutorial-template.html        ← 风格 A 教程模板（基于 template.html 改造）
│   ├── tutorial-template-swiss.html  ← 风格 B 教程模板（基于 template-swiss.html 改造）
│   ├── template.html                 ← 原 PPT 模板（保留供参考，本 skill 不引用）
│   ├── template-swiss.html           ← 原 PPT 模板（保留供参考，本 skill 不引用）
│   ├── screenshot-backgrounds/       ← 截图背景资产（沿用）
│   └── motion.min.js                 ← Motion One 本地副本
├── references/
│   ├── components.md                 ← 风格 A 组件手册（沿用）
│   ├── layouts.md                    ← 风格 A 10 种章节骨架（沿用，语义改为"章节内排版"）
│   ├── layouts-swiss.md              ← 风格 B 22 种章节骨架（沿用）
│   ├── swiss-layout-lock.md          ← 风格 B 版式硬约束（继续适用）
│   ├── themes.md                     ← 风格 A 5 套主题色（沿用）
│   ├── themes-swiss.md               ← 风格 B 4 套主题色（沿用）
│   ├── checklist.md                  ← 质量自检（已加教程模式 P0 section）
│   ├── image-prompts.md              ← Codex 配图（沿用）
│   ├── screenshot-framing.md         ← 截图美化（沿用）
│   └── swiss-map-component.md        ← 瑞士风地图组件（沿用）
└── scripts/
    └── validate-swiss-deck.mjs       ← 瑞士风版式校验（继续适用于风格 B 教程）
```

## 与原 guizang-ppt-skill 的区别（速查表）

| 维度 | 原 PPT skill | 本 Tutorial skill |
|---|---|---|
| 产出 | 横向翻页单 HTML | 纵向滚动单 HTML |
| 触发 | PPT / deck / 分享 / 演讲 | 教程 / 报告 / 学习笔记 / 知识点 |
| 风格选择 | 7 问问用户选 | **bash 强制随机** 1-9 |
| 流程长度 | 7 步 | 4 步 |
| 章节单位 | 一页一题 | 一节一题 |
| 视觉留白 | PPT 风格大留白 | **信息密度优先** |
| 输出位置 | 项目目录 `项目/XXX/ppt/` | **`~/Desktop/学习笔记/`** |
| 翻页机制 | keydown/wheel/touch + ESC 索引 | 自然滚动 + IntersectionObserver + 侧边 TOC |
| 键盘快捷键 | ← → 翻页 / B 静态 / ESC 索引 | B 静态（保留），其余去除 |
| 视觉资产 | 字体/配色/layout/Motion/WebGL | **全部继承，不动一个字符** |
