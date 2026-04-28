# Image Generation Skills

为 Claude / ChatGPT 准备的产品图片 AI 生成 Skill 套件。每个 Skill 专注一种图片用途，共享同一套行业 / 情感 / 质感 / 场景知识库。

## 当前 Skills

| Skill | 用途 | 输出形态 | 状态 |
|-------|------|---------|------|
| **hero-banner-bg** | 产品官网首屏 Hero Banner 背景图 | **每次 2 套 × 桌面+移动 = 4 张** | v1.0-skeleton |
| cover-image | 内容封面图 | 待定 | 规划中 |
| category-image | 分类导航图 | 待定 | 规划中 |

### hero-banner-bg 输出说明

每次生成**强制固定 4 张图**，由 2 套构成：

```
Set 1：
  ├── 桌面 1920×600（Variant A）
  └── 移动 1080×1080（Variant A，主体一致）

Set 2：
  ├── 桌面 1920×600（Variant B）
  └── 移动 1080×1080（Variant B，主体一致）
```

用户**不能修改**：尺寸、数量、是否双端。
用户**可以修改**：行业、文案位置、视觉表现形式、情感、关键词、输出格式（WebP/PNG/JPG）、排除元素。

## 仓库结构

```
image-generation/
├── README.md                              ← 你正在读的文件
│
├── _shared/                               ← 跨 Skill 共享的知识库
│   └── references/
│       ├── enums.md                       ← 行业/情感/文案位置（含双端构图）/输出格式
│       ├── quality-modules.md             ← 11 个图像质感模块（M1–M11）
│       ├── negative-rules.md              ← 通用负向词 + 关键词语义联动
│       └── industries/                    ← 14 个行业的预设和 Scene Variants
│           ├── 00-housing.md
│           ├── 01-mobile-telecom.md
│           ├── 02-finance.md
│           ├── 03-medical.md
│           ├── 04-legal.md
│           ├── 05-education.md
│           ├── 06-local-services.md
│           ├── 07-dining.md
│           ├── 08-auto.md
│           ├── 09-pets.md
│           ├── 10-app-download.md
│           ├── 11-logistics.md
│           ├── 12-templates.md
│           └── 13-astrology.md
│
└── hero-banner-bg/                        ← Hero Banner 背景图 Skill
    ├── SKILL.md                           ← Skill 主控（含 frontmatter + 触发条件）
    ├── agents/
    │   └── openai.yaml                    ← ChatGPT Custom GPT 元信息
    └── references/
        ├── input-spec.md                  ← 输入字段规范（含固定参数说明）
        ├── mode-spec.md                   ← Hero 模式特有规则（双端尺寸、桌面/移动构图）
        ├── prompt-template.md             ← Prompt 8 块拼装 + 双端配对 Variant 规则
        └── output-format.md               ← 用户回复模板（双端配对展示）
```

## 安装与使用

### 在 Claude / Cowork 中

1. 把整个 `image-generation/` 文件夹放到你的 Skills 目录（如 `~/.claude/skills/`）
2. Skill 会通过 `hero-banner-bg/SKILL.md` 中的 `description` frontmatter 自动注册
3. 当你提出"帮我生成 Hero Banner / 产品首屏背景图"这类请求时，Claude 会自动调用本 Skill

### 在 ChatGPT 中（Custom GPT）

1. 创建一个新的 Custom GPT
2. 把 `hero-banner-bg/SKILL.md` 的内容（去掉 frontmatter）粘贴到 **Instructions**
3. 把 `_shared/references/` 和 `hero-banner-bg/references/` 下所有 `.md` 文件上传到 **Knowledge**
4. 参考 `hero-banner-bg/agents/openai.yaml` 配置 GPT 元信息（name / description / conversation starters）
5. 启用 GPT-Image 工具

### 在任何 LLM 对话里手动使用

如果上面两种方式都不可用，可以把 SKILL.md + 所有 references 内容拼接在一起，作为系统提示粘贴到对话开头。

## 当前完成度

### hero-banner-bg

- ✅ 控制层完整（SKILL.md、所有 references）
- ✅ 双端生成机制（桌面+移动）
- ✅ 1/11 Quality Module 完整（M1_real_human）
- ✅ 1/14 行业完整（00-housing，含 1 个 Scene Variant 示例）
- ✅ 1/6 关键词语义联动完整
- ⏳ 待补充：M2–M11、其余 13 个行业、5 类语义联动

### 待补充清单

详见各文件内的 `[待补充]` 标记，优先级建议：

1. 先补 M2–M11 质感模块（解锁所有行业的质感引用）
2. 再批量补 13 个行业的视觉预设和 Scene Variants
3. 最后补关键词语义联动表的剩余 5 类

## 验证可行性

最快测试路径：

```
输入：
行业: 住房 / 房地产
文案区域位置: 左留白
```

预期输出：

```
Set 1：
  ├── 桌面图 1920×600（v0_home_01：客厅+沙发+午后阳光）
  └── 移动图 1080×1080（v0_home_01，构图自动映射为下方留白）

Set 2：
  ├── 桌面图 1920×600（v0_home_01 + 微调，例如 lighting=morning）
  └── 移动图 1080×1080（同上微调，下方留白构图）
```

> 当前住房只有 1 个变体（v0_home_01），所以 Set 2 会在同一变体基础上做 lighting / angle 微调。补完 v0_home_02 和 v0_home_03 后，Set 2 会用真正不同的变体。

## 版本

v1.0-skeleton（2026-04-28）

### 与 v1.0 之前版本的关键差异

- **强制双端输出**：每次产出 2 套 × 桌面+移动 = 4 张图
- **移除用户对尺寸和数量的选择权**：固定 1920×600 + 1080×1080
- **新增桌面 → 移动构图自动映射**：左/右留白在移动版自动转为下方留白

## License

MIT
