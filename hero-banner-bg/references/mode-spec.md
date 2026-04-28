# Hero Banner 模式规则

本文件定义 Hero Banner 这种图片类型的特有规则。其他图片类型（封面图、分类图）将有各自的 mode-spec.md。

## 模式定位

- **用途**：产品官网首屏顶部背景图（Hero Section）
- **典型布局**：通屏宽度 + 较低高度，主体一侧 + 文案一侧
- **使用场景**：官网首页、子产品落地页、活动页

## 输出形态（系统固定）

**每次生成产出 2 套图，共 4 张**。每套包含同一 Scene Variant 的桌面版 + 移动版。

| 套次 | 桌面版 | 移动版 | 主体一致性 |
|------|--------|--------|-----------|
| Set 1 | 1920×600（Variant A） | 1080×1080（Variant A） | 同一变体 |
| Set 2 | 1920×600（Variant B） | 1080×1080（Variant B） | 同一变体 |

> **关键约束**：同一套的桌面和移动版必须使用**同一个 Scene Variant + 同一个 Quality Module + 同一个情感**。仅 Composition 块不同（桌面横版 vs 移动方版）。

## 桌面构图规则（1920×600）

| 文案位置（用户输入） | 焦点位置 | 桌面 Composition Instruction |
|--------------------|---------|------------------------------|
| **左留白** | 右侧 2/3 | composition with clean uncluttered space on the LEFT third for text overlay, main visual subject placed on the right two-thirds, balanced negative space |
| **右留白** | 左侧 2/3 | composition with clean uncluttered space on the RIGHT third for text overlay, main visual subject placed on the left two-thirds, balanced negative space |
| **居中下方留白** | 上方 2/3 | composition with main visual in the upper two-thirds, lower third has soft fade or simple gradient providing clean space for centered text overlay |
| **通屏氛围** | 整体均衡 | full-frame ambient composition, low contrast and low visual density throughout, allowing text to be overlaid anywhere without competing with imagery |

## 移动构图规则（1080×1080）⭐ 关键映射

移动版尺寸为 1:1 方形，**桌面的"左/右留白"在方形上不成立**，必须自动映射为竖向结构。

| 用户输入的文案位置 | 桌面构图 | **移动构图（自动映射）** |
|------------------|---------|------------------------|
| 左留白 | 左 1/3 留白（横向） | **下方 1/3 留白（垂直）** |
| 右留白 | 右 1/3 留白（横向） | **下方 1/3 留白（垂直）** |
| 居中下方留白 | 下方 1/3 留白 | 下方 1/3 留白（保持一致） |
| 通屏氛围 | 整体均衡 | 整体均衡（保持一致） |

### 移动 Composition Instructions（注入移动版 Prompt 的 Block 7）

```
左留白 / 右留白 / 居中下方留白 → 移动版均使用：
square composition with main visual subject in the upper two-thirds, lower third has soft fade or simple gradient providing clean space for text overlay below, centered or asymmetric is fine, optimized for 1:1 mobile hero layout
```

```
通屏氛围 → 移动版使用：
full-frame square ambient composition for 1:1 mobile hero, low contrast and low visual density throughout, allowing text to be overlaid anywhere without competing with imagery
```

### 为什么这样映射

- 桌面是 16:5 横向，左/右留白才有意义（用户先看到一边的视觉，再读另一边的文字）
- 移动是 1:1 方形，**人眼习惯从上往下扫视**，文字应该在下方
- 强行在 1080×1080 上做"左 1/3 留白"，文字区只剩 360 像素宽，标题都放不下
- 上图下文是移动 Hero 行业惯例，转化率最稳

## Hero 特有的硬约束

每次生成都必须满足，写入 prompt 模板的 Block 1 主标识句中：

- 必须是 **"hero banner background image for a website"**（强调用途）
- 必须有可叠文字的负空间
- 不能有 UI 元素、按钮、图标带文字
- 不能是封面图样式（不要"杂志封面"调性）

## 模式特有的负向词

除了通用负向词（见 `_shared/references/negative-rules.md`），Hero 模式额外追加：

```
no magazine cover layout, no posters with title space at top,
no extreme portrait aspect ratio (the desktop output is wide-aspect, not tall)
```

> 注意：移动版 1080×1080 是方形，不是竖版，所以负向词里不排斥竖向。

## 调用 GPT-Image-2 的参数模板

每张图独立调用一次，共 4 次。

### 桌面调用

```yaml
model: gpt-image-2
size: "1920x600"
n: 1
quality: high
output_format: {format}
prompt: <桌面 Prompt 全文>
negative_prompt: <桌面 Negative Prompt>
```

### 移动调用

```yaml
model: gpt-image-2
size: "1080x1080"
n: 1
quality: high
output_format: {format}
prompt: <移动 Prompt 全文>
negative_prompt: <移动 Negative Prompt>
```

## 与其他模式的差异（前瞻）

为未来扩展时对比参考：

| 维度 | hero-banner-bg | cover-image（规划中） | category-image（规划中） |
|------|---------------|----------------------|-------------------------|
| 默认尺寸 | 1920×600（PC）+ 1080×1080（移动） | 1200×630 | 800×800 |
| 双端输出 | ⭐是 | 否 | 否 |
| 文案布局 | 一侧留白为主 | 居中或下方 | 一般无文字叠加 |
| 主视觉 | 通屏氛围 | 强焦点 | 强符号化 |
