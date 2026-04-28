# Quality Module Library（质感模块库）

定义 11 个图像质感模块，每个 Skill 的视觉预设通过 Quality Module ID 引用本文件。

修改任一模块的描述会同步影响所有引用它的预设 —— 这正是模块化的价值。

---

## M1_real_human（真人写实） ✅ 完整示例

**适用预设**：温馨居家场景、社区生活、真人通信场景、家庭通话、真人办公场景、真人医患场景、真人专业场景、真人学习场景、真人服务场景、真人用餐场景、主人陪伴、真人配送场景

**English Description**:

```
photorealistic photography style, natural skin texture and pores,
realistic facial features with proportional anatomy,
natural hand details with five fingers in correct anatomy,
candid lifelike expression (not posed model, not airbrushed),
authentic clothing fabric texture,
natural environmental lighting (window light, ambient interior light, daylight),
shallow depth of field consistent with real camera (50mm or 85mm lens feel),
NO plastic doll-like faces, NO uncanny valley artifacts, NO stiff modeling poses
```

---

## M2_real_object（物体写实摄影）

**适用预设**：真实车辆特写、美食特写、真实宠物场景、温馨家居（宠物）、自然元素（医疗）、包裹意象

**English Description**:

```
[待补充]
建议关键词：product photography style, sharp focus on subject, realistic textures and material details
（fabric, fur, food, paper, metal）, soft natural lighting with gentle shadows, shallow depth of field,
professional composition, true-to-life color reproduction, NO over-processed HDR look,
NO plastic-looking surfaces, NO oversaturated colors
```

---

## M3_real_architecture（建筑写实摄影）

**适用预设**：城市公寓外观、城市街景、公路场景、城市驾驶、城市俯瞰

**English Description**:

```
[待补充]
建议关键词：realistic architectural photography style, accurate perspective and proportions,
natural daylight or golden hour lighting, true-to-life material textures（concrete, glass, wood, brick）,
clean atmospheric depth without excessive haze, NO distorted geometry, NO impossible perspectives,
NO over-rendered CGI feel
```

---

## M4_flat_illustration（扁平矢量插画）

**适用预设**：萌系插画（宠物）、活力插画（餐饮）、复古海报风、抽象地图

**English Description**:

```
[待补充]
建议关键词：flat 2D vector illustration style, clean solid color blocks with subtle gradient shading,
simplified geometric shapes, modern editorial / Dribbble aesthetic, crisp clean vector lines,
limited but harmonious color palette, NO photorealistic textures, NO 3D depth effects,
NO drop shadows mimicking photo lighting
```

---

## M5_handdrawn_illustration（手绘插画）

**适用预设**：手绘插画（教育）、卡通占星插画

**English Description**:

```
[待补充]
建议关键词：hand-drawn illustration style with visible brush or pencil strokes, soft watercolor
or crayon textures, warm imperfect linework, friendly approachable character/scene design,
muted hand-mixed color palette, NO sharp vector edges, NO photographic detail, NO 3D rendering
```

---

## M6_isometric_3d（等距 3D 插画）

**适用预设**：等距 3D 校园

**English Description**:

```
[待补充]
建议关键词：isometric 3D illustration style, 30-degree axonometric projection, clean geometric shapes
with subtle ambient occlusion, modern flat-3D aesthetic, soft pastel or vibrant tech palette,
small detailed elements arranged in clean composition, NO photo-realistic textures,
NO perspective distortion
```

---

## M7_3d_render（3D 渲染）

**适用预设**：抽象建筑几何、抽象 UI 元素、3D 应用图标群

**English Description**:

```
[待补充]
建议关键词：high-quality 3D render style, soft global illumination, glossy or matte material surfaces
with realistic reflections, modern Apple-product / Cinema4D aesthetic, clean studio lighting with
subtle shadows, smooth bezier curves and refined geometry, NO low-poly look, NO obvious CGI artifacts,
NO harsh edges
```

---

## M8_abstract_geometric（抽象几何）

**适用预设**：抽象几何（金融）、数据网格、抽象信号网格、抽象知识图谱、抽象几何线条（法律）、抽象路径线条、抽象几何（模板）

**English Description**:

```
[待补充]
建议关键词：abstract geometric composition, clean sharp lines and pure shapes
（circles, triangles, hexagons, polygons）, flat or subtle gradient fills,
mathematical balanced layout, modern minimal graphic design aesthetic,
NO photographic content, NO realistic objects, NO illustrative characters
```

---

## M9_abstract_atmospheric（抽象氛围光效）

**适用预设**：神秘星空、渐变魔法光效、渐变光效、抽象速度线

**English Description**:

```
[待补充]
建议关键词：abstract atmospheric composition, soft glowing light effects, gradient color fields,
nebula-like clouds, particle bokeh, ethereal dreamy mood, smooth color blending and depth,
cinematic lighting, NO sharp geometric edges, NO photographic subjects, NO clear figurative content
```

---

## M10_minimal_modern（极简现代）

**适用预设**：极简手机意象、极简书架背景、极简医疗意象、极简文档堆叠、极简图标网格、网格排列、极简几何（App）

**English Description**:

```
[待补充]
建议关键词：minimal modern design, generous negative space, clean simple subject placement,
soft uniform lighting, restrained color palette（1–3 colors）, contemporary editorial sensibility,
NO clutter, NO excessive detail, NO competing visual elements
```

---

## M11_symbol_icon（符号图标）

**适用预设**：安全符号（金融）、经典法律符号、抽象健康符号、占星符号

**English Description**:

```
[待补充]
建议关键词：clean symbolic iconography composition, single hero symbol or small icon cluster as
focal point, soft glow or subtle shadow elevation, balanced negative space around the symbol,
clear semantic readability, NO photo-realistic textures, NO complex backgrounds, NO competing details
```

---

## 补充优先级

模块编号 | 优先级 | 理由
---------|--------|------
M1 | ✅ 已完成 | 真人类预设最多（12 个），影响面最广
M2 | 🔴 高 | 物体写实第二大类
M3 | 🔴 高 | 建筑/街景类
M8 | 🔴 高 | 抽象几何类（金融、模板、物流多用）
M9 | 🟡 中 | 氛围类（占星、汽车）
M11 | 🟡 中 | 符号类（金融、法律核心预设）
M7 | 🟡 中 | 3D 类（App 核心预设）
M10 | 🟡 中 | 极简类
M4 | 🟢 低 | 扁平插画（用得少）
M5 | 🟢 低 | 手绘（仅 2 个预设用）
M6 | 🟢 低 | 等距 3D（仅 1 个预设用）
