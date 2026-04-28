# 08 汽车

## 元数据

```yaml
id: 8
name_zh: 汽车
slug: auto
default_preset: 公路场景
default_emotion: 专业可信
status: skeleton
```

## 视觉表现形式预设（4 个）

| 视觉预设 | Quality Module | Visual Preset Description |
|---------|----------------|---------------------------|
| ★公路场景 | M3_real_architecture | [待补充] |
| 真实车辆特写 | M2_real_object | [待补充] |
| 抽象速度线 | M9_abstract_atmospheric | [待补充] |
| 城市驾驶 | M3_real_architecture | [待补充] |

## Scene Variant Pool

### 预设 1：公路场景 (M3_real_architecture)

```
[待补充：写 2–3 个变体]
建议方向：
- 公路+山景+晚霞
- 林间公路+绿树+晨光
- 海边公路+蓝天+延伸
```

### 预设 2：真实车辆特写 (M2_real_object)

```
[待补充]
建议方向：
- 引擎盖+反光+棚拍
- 车轮+轮毂+特写
```

### 预设 3：抽象速度线 (M9_abstract_atmospheric)

```
[待补充]
建议方向：
- 光线+流动+夜色
- 矢量+发散+紫蓝
```

### 预设 4：城市驾驶 (M3_real_architecture)

```
[待补充]
建议方向：
- 城市天际线+暮色+车头
- 隧道+灯光+延伸
```

## 行业特殊规则

- 汽车工具类（保险查询、二手车）建议默认走"专业可信"而非"沉稳厚重"，避免高端豪车调性
- 涉及具体车型时，关键词加上车型名（如"SUV、轿车、电动车"）让模型更精准
