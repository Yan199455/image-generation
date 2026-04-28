# 枚举定义

跨 Skill 共享的枚举值。任何 Skill 引用这些枚举时直接 Read 本文件。

## 行业枚举（14 项）

| ID | 中文名 | 英文 slug |
|----|--------|-----------|
| 0 | 住房 / 房地产 | housing |
| 1 | 手机 / 通信 / 补贴项目 | mobile-telecom |
| 2 | 金融 / 银行 / 信用卡 / 税务 | finance |
| 3 | 医疗 / 医院 / 牙科 | medical |
| 4 | 法律 | legal |
| 5 | 教育 / 驾考 | education |
| 6 | 本地服务 / 政府机构 / 目录查询 | local-services |
| 7 | 餐饮 / 优惠券 / 消费信息 | dining |
| 8 | 汽车 | auto |
| 9 | 宠物 | pets |
| 10 | App / APK / 工具下载 | app-download |
| 11 | 物流追踪 | logistics |
| 12 | 模板文档 | templates |
| 13 | 占星娱乐 | astrology |

每个行业的详细预设和 Scene Variants 见 `industries/<id>-<slug>.md`。

## 情感表达枚举（6 项）

每条情感包含 Mood Keywords（注入 Prompt Block 5）和 Color Palette（注入 Block 6）。

| 情感（中文） | Mood Keywords | Color Palette |
|-------------|---------------|---------------|
| **朴实可信** | humble, trustworthy, grounded, authentic, down-to-earth | warm beige, soft cream, muted earth tones, gentle natural lighting |
| **专业可信** | professional, reliable, dependable, composed, established | deep navy, forest green, slate gray, silver accents |
| **温暖治愈** | warm, caring, gentle, comforting, soothing | warm peach, cream, soft pink, golden hour light |
| **简洁清爽** | clean, minimal, modern, fresh, airy, spacious | pure white, light gray, cool blue, fresh mint |
| **活力激发** | energetic, vibrant, dynamic, motivating, exciting | vivid orange-red, bright yellow, lively green, high saturation |
| **神秘梦幻** | mystical, dreamy, ethereal, otherworldly, surreal | deep purple, indigo, starry black with gold sparkle |

## 文案区域位置枚举（4 项）

每条位置包含焦点描述和**桌面/移动两套** Composition Instruction。

| 文案位置 | 桌面焦点 | 桌面 Composition Instruction | 移动焦点 | 移动 Composition Instruction |
|---------|---------|------------------------------|---------|------------------------------|
| **左留白** | 右侧 2/3 | composition with clean uncluttered space on the LEFT third for text overlay, main visual subject placed on the right two-thirds, balanced negative space | 下方 1/3 | square composition with main visual in the upper two-thirds, lower third has soft fade or simple gradient providing clean space for text overlay below, optimized for 1:1 mobile hero |
| **右留白** | 左侧 2/3 | composition with clean uncluttered space on the RIGHT third for text overlay, main visual subject placed on the left two-thirds, balanced negative space | 下方 1/3 | square composition with main visual in the upper two-thirds, lower third has soft fade or simple gradient providing clean space for text overlay below, optimized for 1:1 mobile hero |
| **居中下方留白** | 上方 2/3 | composition with main visual in the upper two-thirds, lower third has soft fade or simple gradient providing clean space for centered text overlay | 上方 2/3 | square composition with main visual in the upper two-thirds, lower third has soft fade providing clean space for centered text overlay |
| **通屏氛围** | 整体均衡 | full-frame ambient composition, low contrast and low visual density throughout, allowing text to be overlaid anywhere without competing with imagery | 整体均衡 | full-frame square ambient composition for 1:1 mobile hero, low contrast and low visual density throughout, allowing text to be overlaid anywhere without competing with imagery |

> 注：移动版"左留白 / 右留白" → 自动映射为"下方留白"，因为 1:1 方形不适合横向留白。

## 输出形态（系统固定，不是用户输入）

```yaml
sets_per_generation: 2          # 每次固定 2 套
platforms_per_set: [desktop, mobile]   # 每套必出双端
total_images: 4                 # 2 × 2 = 4 张

desktop_size: 1920x600          # 横向 16:5
mobile_size: 1080x1080          # 1:1 方形
```

> 用户不可修改这些参数。试图修改时按 `output-format.md` 的"用户试图修改固定参数时的提示"回应。

## 输出格式枚举（3 项）⭐ 用户可选

| 格式 | 用途 | 备注 |
|------|------|------|
| **WebP** ⭐默认 | 现代浏览器、文件小、质量高 | 推荐用于网站 Hero |
| **PNG** | 兼容性最好、支持透明 | 文件较大 |
| **JPG** | 文件最小、不支持透明 | 适用于纯背景，无需透明 |
