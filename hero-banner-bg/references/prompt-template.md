# Prompt 合成模板

定义 hero-banner-bg Skill 的 GPT-Image-2 Prompt 拼装结构、Variant 选择规则和工作流细节。

## 输出形态总览

每次生成 = **2 套图 × 2 端 = 4 张图**。

```
Variant A：
  ├── 桌面 Prompt (1920×600，桌面构图)
  └── 移动 Prompt (1080×1080，移动构图)

Variant B：
  ├── 桌面 Prompt (1920×600，桌面构图)
  └── 移动 Prompt (1080×1080，移动构图)
```

**Variant A 和 B 的桌面/移动 Prompt 共享 7 个块（1-6 + 8）**，仅 **Block 7 Composition** 不同。

## 正向 Prompt 拼装顺序（8 块）

按以下顺序拼装，每块之间用句号或逗号分隔，整体保持自然语言流畅。

```
[Block 1: 主标识句 - Hero 模式硬要求]
A high-quality hero banner background image for a website.

[Block 2: Quality Module - 来自 _shared/references/quality-modules.md]
{{quality_module_full_text}}

[Block 3: Visual Preset Description - 来自 _shared/references/industries/<行业>.md]
{{visual_preset_description}}

[Block 4: Scene Variant Subject - 来自当前预设的变体池]
{{scene_variant_english_description}}

[Block 5: Mood Keywords - 来自 _shared/references/enums.md 的情感表]
Mood and tone: {{emotion_mood_keywords}}.

[Block 6: Color Palette - 来自 _shared/references/enums.md 的情感表]
Color palette: {{emotion_color_palette}}.

[Block 7: Composition - 桌面与移动不同，详见 mode-spec.md]
{{composition_instruction}}.

[Block 8: User Keywords (条件块，仅当用户提供时插入)]
Additional content focus: {{user_keywords_translated_to_english}}.

[Technical]
Aspect ratio: {{width}}x{{height}}.
```

### 桌面 Prompt 的 Block 7 来源

读 `mode-spec.md` 的"桌面构图规则"表，按用户的"文案区域位置"取对应英文。

### 移动 Prompt 的 Block 7 来源

读 `mode-spec.md` 的"移动构图规则"表，按"自动映射"列取对应英文。

## 负向 Prompt 拼装

桌面和移动版**共享相同负向词**（除非未来移动有特殊负向需求）。

```
{{universal_negative_keywords}},
{{hero_mode_specific_negatives}},
{{auto_linked_negatives_from_user_keywords}},
{{user_excluded_elements_translated}}
```

来源：

- `universal_negative_keywords`：来自 `_shared/references/negative-rules.md` 的"通用负向词"
- `hero_mode_specific_negatives`：来自 `mode-spec.md` 的"模式特有负向词"
- `auto_linked_negatives_from_user_keywords`：来自 `_shared/references/negative-rules.md` 的"关键词语义联动"，按命中追加
- `user_excluded_elements_translated`：把用户输入的"排除元素"逐项翻译成英文

## GPT-Image-2 调用参数（每张图独立调用）

### 桌面调用模板

```yaml
model: gpt-image-2
size: "1920x600"
n: 1
quality: high
output_format: {format}
prompt: <桌面 Prompt 全文>
negative_prompt: <负向 Prompt 全文>
```

### 移动调用模板

```yaml
model: gpt-image-2
size: "1080x1080"
n: 1
quality: high
output_format: {format}
prompt: <移动 Prompt 全文>
negative_prompt: <负向 Prompt 全文>
```

> 共 4 次调用：Variant A 桌面、Variant A 移动、Variant B 桌面、Variant B 移动。

## Scene Variant 选择规则（双端配对版）

设 `pool = 当前预设的变体池`。

```
Step A: 关键词匹配检查
  for each user_keyword:
    for each variant in pool:
      if 语义匹配(user_keyword, variant.tag):
        锁定该变体作为 Variant A
        break

Step B: 选择 Variant B
  if Variant A 已锁定:
    Variant B = pool 里和 A 不同的随机变体
  else:
    Variant A = pool 里随机选 1 个
    Variant B = pool 里和 A 不同的随机变体

Step C: 池容量不足时
  if pool.size == 1:
    Variant A 和 B 都用唯一变体，但 B 加随机微调修饰：
      - lighting variation: morning / afternoon / golden hour
      - angle variation: lower angle / wider angle
    把这些微调追加到 B 的 Block 4 末尾

Step D: 双端配对（关键）
  for variant in [A, B]:
    生成桌面 Prompt（用桌面 Block 7）
    生成移动 Prompt（用移动 Block 7）
    其他 7 个块在两端完全一致
```

### 语义匹配的判断方式

不要求字面一致，按"语义近似"判断。例如：

- 用户关键词"金毛"→ 命中变体 tag"金毛幼犬+客厅+玩耍"（直接匹配）
- 用户关键词"普通家庭"→ 命中变体 tag"客厅+沙发+午后阳光"（"普通家庭"语义贴近"家居生活"）
- 用户关键词"驾考"→ 命中变体 tag"驾考学员+车内+教练"（直接匹配）

匹配置信度低时，不强行锁定，让该关键词只走"内容修饰"路径（进入 Block 8）。

## 完整工作流（10 步详细版）

### Step 1：输入收集与校验

- 从用户消息中解析输入字段
- 必填项缺失 → 追问用户，不要继续
- 用户若试图指定尺寸 / 数量 → 礼貌告知 v1.0 强制固定，继续按默认执行

### Step 2：默认值填充

- 行业 slug 映射（见 SKILL.md 的"行业 → 文件名映射"表）
- 读 `_shared/references/industries/<slug>.md` 取出 `default_preset` 和 `default_emotion`
- 输出格式默认 WebP

### Step 3：加载行业知识

- 读 `_shared/references/industries/<slug>.md` 取出当前预设的：
  - Quality Module ID
  - Visual Preset Description（英文）
  - Scene Variant 池

### Step 4：加载质感模块

- 读 `_shared/references/quality-modules.md`
- 取出当前预设绑定的 Mx 模块完整英文文本

### Step 5：变体选择（双端配对版）

- 按"Variant 选择规则"选出 Variant A 和 Variant B
- 关键词匹配优先

### Step 6：负向词组装

- 读 `_shared/references/negative-rules.md` 取通用负向词
- 读 `mode-spec.md` 取模式特有负向词
- 对用户每个关键词做语义匹配，命中即追加联动负向词
- 追加用户的"排除元素"（翻译成英文）

### Step 7：双端 Prompt 合成

- 读 `mode-spec.md` 的"桌面构图规则"和"移动构图规则"
- 对 Variant A 拼装：桌面 Prompt + 移动 Prompt
- 对 Variant B 拼装：桌面 Prompt + 移动 Prompt
- 共 4 个完整 Prompt

### Step 8：在调用前向用户展示

按 `output-format.md` 的格式：

- 输入解析摘要
- 2 个 Variant 的 tag
- Variant A 桌面 Prompt 全文（作为代表展示）

### Step 9：执行生成（4 次调用）

按以下顺序调用 GPT-Image-2：

1. Variant A 桌面图（1920×600）
2. Variant A 移动图（1080×1080）
3. Variant B 桌面图（1920×600）
4. Variant B 移动图（1080×1080）

### Step 10：双端配对交付

按 `output-format.md` 的"双端配对展示模板"输出：

- Set 1：桌面 + 移动 并排展示（带 Variant tag）
- Set 2：桌面 + 移动 并排展示（带 Variant tag）
- 操作提示
