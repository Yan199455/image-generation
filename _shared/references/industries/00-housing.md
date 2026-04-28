# 00 住房 / 房地产 ✅ 完整示例

本文件是 14 个行业文件的**格式标准模板**。其他 13 个行业按本文件结构补充内容。

## 元数据

```yaml
id: 0
name_zh: 住房 / 房地产
slug: housing
default_preset: 温馨居家场景
default_emotion: 朴实可信
status: complete-skeleton  # 含 1 个完整 Scene Variant 示例
```

## 视觉表现形式预设（4 个）

★ = 该行业默认预设。

| 视觉预设 | Quality Module | Visual Preset Description（不含具体主体） |
|---------|----------------|-------------------------------------------|
| ★温馨居家场景 | M1_real_human | a warm cozy residential interior with inviting atmosphere |
| 城市公寓外观 | M3_real_architecture | a residential mid-rise apartment building exterior view |
| 抽象建筑几何 | M7_3d_render | abstract 3D architectural composition with clean geometry |
| 社区生活 | M1_real_human | a friendly walkable neighborhood scene |

## Scene Variant Pool（场景变体池）

每个预设挂 2–3 个变体。变体格式：

```
variant_id | 中文 tag（用于关键词匹配） | English Subject Description
```

> **双端共用**：同一个 variant 同时用于桌面版（1920×600）和移动版（1080×1080）的 Block 4，仅 Block 7 构图不同。

### 预设 1：温馨居家场景 (M1_real_human)

```
v0_home_01 | 客厅+沙发+午后阳光 | a cozy mid-class living room with linen sofa and bookshelf, soft afternoon sunlight, indoor plants on coffee table
```

```
[待补充：再补 2 个变体]
建议方向：
- v0_home_02：厨房+早餐+家庭场景，例如"warm bright kitchen with breakfast on wooden table, ordinary family member pouring coffee"
- v0_home_03：卧室+晨光+整洁，例如"tidy modest bedroom with morning light through curtains, made bed and reading chair"
```

> 当前只有 1 个变体时，Skill 会让 Set 1 用 v0_home_01 原版，Set 2 用 v0_home_01 + 随机微调（lighting / angle / time of day）。补完 v0_home_02 后两套就会真正用不同变体。

### 预设 2：城市公寓外观 (M3_real_architecture)

```
[待补充：写 2–3 个变体]
建议方向：
- 多层公寓+晴天+绿树
- 红砖公寓+傍晚+暖光
- 玻璃幕墙+现代感
```

### 预设 3：抽象建筑几何 (M7_3d_render)

```
[待补充：写 2–3 个变体]
建议方向：
- 立方体+空间感+柔光
- 阶梯+光影+极简
- 弧线建筑+渐变背景
```

### 预设 4：社区生活 (M1_real_human)

```
[待补充：写 2–3 个变体]
建议方向：
- 街区步行+居民+背影
- 社区公园+长椅+老人
- 邻里互动+温暖氛围
```

## 行业特殊规则

无特殊规则。

> 备注：当用户关键词命中"普通 / 朴素 / 平价"语义时，会自动触发"第 1 类"语义联动负向词（见 `_shared/references/negative-rules.md`），避免画面看起来豪华。这是住房行业最常见的场景。

## 双端输出说明

调用本预设生成时：

```
桌面（1920×600）：
  Block 7 = 桌面构图（按用户文案位置查 mode-spec.md 的桌面表）

移动（1080×1080）：
  Block 7 = 移动构图（按用户文案位置查 mode-spec.md 的移动表，
           左/右留白自动映射为下方留白）

其余 7 个块（Block 1-6 + 8）桌面和移动完全相同。
```
