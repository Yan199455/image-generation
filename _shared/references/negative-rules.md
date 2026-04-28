# Negative Rules（负向词规则）

定义所有 Skill 共享的：

1. 通用负向词（每次生成都包含）
2. 关键词语义 → 自动追加负向词的联动表

---

## 通用负向词（Universal Negative Keywords）

每次生成都默认注入到 negative prompt 中：

```
no text, no letters, no words, no typography, no captions,
no logo, no brand mark, no watermark, no signature,
no UI buttons, no CTA elements, no icons with text,
no low quality, no blurry, no distorted faces, no extra fingers,
no cluttered composition (unless requested),
no AI artifacts, no oversaturated colors (unless emotion = 活力激发)
```

> Skill 实现时把这段直接拼接到负向 Prompt 的开头。

---

## 关键词语义 → 自动排除项联动

当用户输入的关键词命中以下任一语义类型，**自动追加对应负向词**到 Prompt（无需用户手填）。

匹配方式为"语义相似"而非"字面一致"。例如关键词"低收入"语义贴近"经济 / 平价"，应触发第 1 类联动。

---

### 第 1 类：朴素 / 平价 / 经济 / 普通 / 实惠 / 补贴 ✅ 完整示例

**触发词示例**（语义匹配，不要求字面一致）：

```
经济、平价、补贴、普通家庭、低收入、实惠、性价比、便宜、亲民、大众、
经济适用、低价、节俭、过日子、省钱
```

**追加的英文负向词**：

```
luxury, opulent, gold, marble, sports car, mansion, expensive jewelry, high-end fashion,
designer brand, premium materials, opulent decor, wealthy lifestyle
```

**典型场景**：穷人住房、补贴项目、平价餐饮、经济型保险

---

### 第 2 类：儿童 / 亲子 / 教育

**触发词示例**：

```
[待补充]
建议方向：儿童、孩子、亲子、家庭、学生、小学生、青少年、童年、母婴
```

**追加的英文负向词**：

```
[待补充]
建议方向：violence, dark moods, scary imagery, suggestive content, alcohol, smoking, weapons
```

---

### 第 3 类：健康 / 医疗

**触发词示例**：

```
[待补充]
建议方向：健康、医疗、康复、治疗、预防、保健
```

**追加的英文负向词**：

```
[待补充]
建议方向：blood, wounds, illness imagery, dark gloomy atmosphere, anxiety-inducing,
hospital bed with patient, medical distress
```

---

### 第 4 类：安全 / 信任 / 隐私

**触发词示例**：

```
[待补充]
建议方向：安全、隐私、保护、信任、加密、防护
```

**追加的英文负向词**：

```
[待补充]
建议方向：shadowy figures, surveillance feel, oppressive atmosphere, dark alleys,
hooded figure, hacker stereotype
```

---

### 第 5 类：老人 / 长者

**触发词示例**：

```
[待补充]
建议方向：老人、长者、银发、退休、养老
```

**追加的英文负向词**：

```
[待补充]
建议方向：depressing, isolation, frail body, hospital bed, sad expression, loneliness imagery
```

---

### 第 6 类：宠物 / 萌

**触发词示例**：

```
[待补充]
建议方向：宠物、可爱、萌、小动物、毛孩子
```

**追加的英文负向词**：

```
[待补充]
建议方向：scary animals, aggressive poses, dirty environment, sick animals, distressed pet
```

---

## 联动叠加规则

- 多个关键词同时命中不同类，**所有命中的负向词都追加**
- 重复负向词去重
- 用户的"排除元素"输入**追加到所有联动负向词之后**
- 最终顺序：通用负向词 → 联动负向词 → 用户排除元素

## 实现伪代码

```python
def assemble_negative_prompt(user_keywords, user_excluded_elements, mode_specific_negatives):
    negatives = []
    negatives.extend(UNIVERSAL_NEGATIVES)
    negatives.extend(mode_specific_negatives)  # 来自 mode-spec.md

    for keyword in user_keywords:
        for category in SEMANTIC_LINKAGE_TABLE:
            if semantic_match(keyword, category.trigger_words):
                negatives.extend(category.negative_words)

    for excluded in user_excluded_elements:
        negatives.append(translate_to_english(excluded))

    return deduplicate(negatives)
```
