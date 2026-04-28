---
name: hero-banner-bg
description: 当用户请求生成、规划、重生成或优化产品官网首屏 Hero Banner 背景图时使用本 Skill。每次产出固定 2 套图，每套含桌面（1920×600）和移动（1080×1080）双端版本，共 4 张。读取用户输入（行业、文案位置等），按 _shared/references 和 hero-banner-bg/references 中的行业预设、场景变体、质感模块、构图规则和负向词规则合成图片 Prompt；若当前环境支持图片生成（如 GPT-Image-2），调用图片工具生成；否则输出可复制的英文生成 Prompt。仅生成纯背景图，不含文字、Logo、CTA。
---

# Hero Banner BG Generator

为产品官网首屏生成 Hero Banner 背景图（不含文字、Logo、CTA），**每次出 2 套 × 双端 = 4 张图**。

## 何时调用本 Skill

**应该调用**：

- 用户明确要求"生成首屏图 / Hero Banner / 网站头图 / 产品官网背景图"
- 用户已提供了"行业"或"产品类型"，意图是为产品官网首屏配图
- 用户对前一次结果说"重新生成 / 换风格 / 优化背景图 / 换一版"
- 用户描述了产品定位（如"我做驾考产品"）并请求首屏视觉

**不应调用**：

- 用户要文字版海报、宣传页（这些需要文字合成，不属于纯背景图）
- 用户要 Logo / CTA 按钮 / 完整营销物料
- 用户要广告投放素材（不同尺寸规范）
- 用户要内容封面图、分类图（属于其他 Skill）

## 输出形态（强制固定）

**每次生成产出 4 张图，由系统固定，用户不可改**：

```
Set 1：
  ├── 桌面 1920×600（Variant A）
  └── 移动 1080×1080（Variant A，主体一致）

Set 2：
  ├── 桌面 1920×600（Variant B）
  └── 移动 1080×1080（Variant B，主体一致）
```

如果用户试图指定数量、尺寸、只要桌面 / 只要移动，按 `references/output-format.md` 中的"用户试图修改固定参数时的提示"友好告知 v1.0 强制固定。

## 工作流（按需读取 reference 文件）

### Step 1：校验输入

读 `references/input-spec.md` 了解输入规范。

- **必填项**：行业、文案区域位置
- 任一必填项缺失 → 直接向用户追问，不要猜测
- 关键词数量超过 3 → 只取前 3 个并提示用户

### Step 2：默认值填充

读 `references/input-spec.md` 中的默认值表，对未填项静默填充：

- 视觉表现形式 → 该行业的★默认预设
- 情感表达 → 该行业的默认情感
- 输出格式 → WebP

行业默认套餐查表见 `_shared/references/industries/<行业-slug>.md` 顶部的元数据字段。

### Step 3：加载行业知识

根据用户的"行业"，用下方"行业 → 文件名映射"查到 slug，读 `_shared/references/industries/<slug>.md`：

- 取出该行业的 4 个视觉预设和它们绑定的 Quality Module ID
- 取出当前选定预设的 Scene Variant 池

### Step 4：加载质感模块

读 `_shared/references/quality-modules.md`，取出当前预设绑定的 Mx 模块完整英文描述。

### Step 5：选择 2 个 Scene Variant（双端共享）

按 `references/prompt-template.md` 中的"Variant 选择规则（双端配对版）"：

- 用户关键词命中某变体 tag → 锁定该变体为 Variant A
- Variant B：从池里随机选另一个不同变体
- 池容量不足时，B 用同变体 + 随机微调（lighting / angle / time）

**关键约束**：同一 Variant 的桌面和移动版必须用**完全相同的变体描述**，仅构图（Block 7）不同。

### Step 6：负向词组装

读 `_shared/references/negative-rules.md`：

- 注入"通用负向词"
- 对用户关键词做语义匹配，命中"语义联动表"则追加对应负向词
- 用户的"排除元素"翻译为英文追加

桌面和移动**共享相同负向词**。

### Step 7：合成 4 个 Prompt

读 `references/prompt-template.md` 和 `references/mode-spec.md`，按 8 块结构拼装：

- Variant A 桌面 Prompt（用桌面 Block 7）
- Variant A 移动 Prompt（用移动 Block 7，自动映射构图）
- Variant B 桌面 Prompt
- Variant B 移动 Prompt

### Step 8：执行生成（4 次调用）

判断当前环境：

- **若有图片生成工具可调用**（如 GPT-Image-2 / Imagen / DALL-E）→ 顺序调用 4 次：
  1. Variant A 桌面（size: 1920x600）
  2. Variant A 移动（size: 1080x1080）
  3. Variant B 桌面（size: 1920x600）
  4. Variant B 移动（size: 1080x1080）
- **若没有**：
  - 输出 4 个完整英文 Prompt + 调用参数
  - 提示用户复制到任何支持 GPT-Image-2 的平台执行

按 `references/output-format.md` 的"双端配对展示模板"回复用户。

## 行业 → 文件名映射

| 用户输入的行业 | 文件 slug |
|---------------|-----------|
| 住房 / 房地产 | `00-housing` |
| 手机 / 通信 / 补贴项目 | `01-mobile-telecom` |
| 金融 / 银行 / 信用卡 / 税务 | `02-finance` |
| 医疗 / 医院 / 牙科 | `03-medical` |
| 法律 | `04-legal` |
| 教育 / 驾考 | `05-education` |
| 本地服务 / 政府机构 / 目录查询 | `06-local-services` |
| 餐饮 / 优惠券 / 消费信息 | `07-dining` |
| 汽车 | `08-auto` |
| 宠物 | `09-pets` |
| App / APK / 工具下载 | `10-app-download` |
| 物流追踪 | `11-logistics` |
| 模板文档 | `12-templates` |
| 占星娱乐 | `13-astrology` |

> 模糊匹配规则：用户输入只要包含行业关键词中任一即可命中（如"我做驾考的"→ 命中"教育 / 驾考"→ slug 为 `05-education`）。

## 关键约束（Hard Rules）

- **画面禁止出现**：文字 / 字母 / Logo / 品牌标识 / 水印 / CTA 按钮 / 带文字的图标
- **每次输出固定 2 套 × 双端 = 4 张**，用户不可改这个数量和尺寸
- **同一套的桌面和移动必须主体一致**，仅构图不同
- **不要把整个知识库灌进上下文**，按需 Read 对应文件即可
- **Prompt 一律用英文**写给 GPT-Image-2，但和用户的对话用中文（除非用户用英文）
- **必填项缺失时主动追问**，不要用"猜测的默认值"代替

## 当前完成度提示

本 Skill 当前为 v1.0-skeleton，仅 `00-housing` 行业完整。其他行业的 reference 文件存在但内容为 `[待补充]`。

如果用户选择了未完整的行业，按以下方式处理：

1. 仍然按上述工作流读取该行业文件
2. 如果发现关键内容（视觉预设描述、Scene Variant）是 `[待补充]`，向用户说明该行业还未填充完整，并询问是否继续：
   - 继续 → 用 Quality Module + 通用预设描述兜底，输出能用但泛化的图
   - 切换到 `00-housing` 测试 → 引导用户先用住房行业验证流程

## Skill 结构索引

```
hero-banner-bg/
├── SKILL.md                        ← 你正在读
├── agents/openai.yaml              ← ChatGPT Custom GPT 元信息
└── references/
    ├── input-spec.md               ← 输入规范（含固定参数说明）
    ├── mode-spec.md                ← Hero 特有：尺寸 / 桌面/移动构图映射
    ├── prompt-template.md          ← Prompt 8 块拼装 + 双端配对 Variant 规则
    └── output-format.md            ← 用户回复模板（双端配对展示）

../_shared/references/              ← 跨 Skill 共享
├── enums.md                        ← 行业 / 情感 / 位置（含双端构图） / 格式枚举
├── quality-modules.md              ← 11 个质感模块
├── negative-rules.md               ← 通用负向词 + 关键词语义联动
└── industries/                     ← 14 个行业知识
    └── <00-13>-<slug>.md
```
