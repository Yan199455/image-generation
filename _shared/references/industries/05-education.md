# 05 教育 / 驾考

## 元数据

```yaml
id: 5
name_zh: 教育 / 驾考
slug: education
default_preset: 真人学习场景
default_emotion: 温暖治愈
status: skeleton
```

## 视觉表现形式预设（4 个）

| 视觉预设 | Quality Module | Visual Preset Description |
|---------|----------------|---------------------------|
| ★真人学习场景 | M1_real_human | [待补充] |
| 手绘插画 | M5_handdrawn_illustration | [待补充] |
| 抽象知识图谱 | M8_abstract_geometric | [待补充] |
| 等距 3D 校园 | M6_isometric_3d | [待补充] |

## Scene Variant Pool

### 预设 1：真人学习场景 (M1_real_human)

```
[待补充：写 2–3 个变体]
建议方向：
- 学生+笔记本+台灯
- 驾考学员+车内+教练
- 在线学习+笔记本电脑
```

### 预设 2：手绘插画 (M5_handdrawn_illustration)

```
[待补充]
建议方向：
- 黑板+粉笔+卡通学生
- 书+苹果+水彩调色板
```

### 预设 3：抽象知识图谱 (M8_abstract_geometric)

```
[待补充]
建议方向：
- 节点+连线+发光
- 思维导图+分支+柔色
```

### 预设 4：等距 3D 校园 (M6_isometric_3d)

```
[待补充]
建议方向：
- 教学楼+树+小人
- 图书馆+书桌+学生
```

## 行业特殊规则

- 涉及儿童 / 学生 / 亲子等关键词时会触发第 2 类语义联动，自动排除暴力、阴暗等元素
- 驾考类建议默认走"真人学习场景"而非"等距 3D 校园"，因为驾考用户更看重"真实可学"
