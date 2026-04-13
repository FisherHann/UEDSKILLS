# Panel (Bottom Sheet)

> 底部弹出面板（复合组件，用 AutoLayout 拼装）
> Updated: 2026-04-01
> **设计任何 Panel 前必读此文档。**

---

## Panel 类型

| 类型 | 用途 | 关闭后行为 |
|------|------|-----------|
| **Type 1: 任务/确认** | 用户在面板内完成配置、编辑或确认（含 input/selector/switch 等控件） | 面板关闭，留在当前页 |
| **Type 2: 入口/聚合** | 多个入口选项，用户选一个方向（方法、账户、模式），无表单填写 | 面板关闭 → 进入新页面/切换上下文 |
| **Type 3: 解释/说明** | 解释专业概念或功能规则，通常由 `?` 图标或下划线文字触发 | 用户读完返回原任务 |

---

## 整体结构

```
Panel Root (vertical, w=393, h=auto, itemSpacing=0, bg=Bg_Float, cornerRadius=[16,16,0,0], shadow, clipsContent=true)
  ├── Top Area (paddingBottom=8)
  │     └── Column (itemSpacing=16)
  │           ├── Drag Handle + Header Group (paddingTop=12, itemSpacing=12)
  │           │     ├── 1. Drag Handle — 组件实例 (componentKey)
  │           │     └── 2. Header (标题栏)
  │           └── 3. Content Area — 任意内容（paddingH=20, paddingV=16）
  └── Button Area + iOS Home Bar
        ├── 4. Button (paddingT=16, paddingH=20, paddingB=8)
        └── 5. iOS Home Bar (h=34)
```

### Panel Shadow

Panel 自带阴影：`color: #121214 (static_Black), opacity: 10%, blurRadius: 24, offset: (0,0)`。
使用 Button_Group 组件时不需手动添加，仅在自定义拼装 Panel 时注意。

### 区块间距

| 从 → 到 | 间距 | 说明 |
|---------|------|------|
| Drag Handle → Header | **12** | 同属 Top 分组，组内 itemSpacing |
| (DragHandle+Header) → Content | **16** | Top Area 内 column itemSpacing |
| Content paddingTop/Bottom | **16** | Content 区域自身 padding |
| Content paddingLeft/Right | **20** | 与页面标准一致 |
| Content → Button Area | **16** | Button Area paddingTop (Button_Group 内置) |
| Button Area → iOS Home Bar | **0** | Button_Group 内置 Home Bar |

---

## 各区块详细规范

### 1. Drag Handle（拖拽指示条）

**始终作为第一个子元素，必须使用组件实例，禁止手绘 rect。**

- **componentKey**: `8ba02c67aec6cf3dee2f96822216e2fccfa8dd45`
- 所在分组 paddingTop=12，与 Header 间距=12

```jsonc
{
  "type": "instance",
  "componentKey": "8ba02c67aec6cf3dee2f96822216e2fccfa8dd45",
  "name": "Panel Handle",
  "layoutSizingHorizontal": "FILL"
}
```

> **⚠️ 禁止手绘**: 不要用 `"type": "rect", "width": 40, "height": 4` 手绘拖拽条。必须用上面的组件实例。

### 2. Header（标题栏）— **可选**

**Header 不是必须的。** 当 Panel 无标题时（如交易面板、说明面板），Handle Group 内只有 Drag Handle，无 Header 子节点。

| 场景 | Header 存在？ | Handle Group 子节点 |
|------|:---:|------|
| 有标题（大多数 Panel） | YES | Drag Handle + Header |
| 无标题（交易面板、说明面板等） | NO | 仅 Drag Handle |

**有 Header 时：**

```
AutoLayout (horizontal, w=fill, h=auto, padding=[0, 20, 0, 20], align=center, justify=space-between)
  ├── 标题文字 (18px, weight=600, lineHeight=26, T1)
  └── [全屏面板] 关闭图标 (24×24, T1)
```

- 标题: **18px Semi Bold**（不是 16 也不是 20）
- 半屏面板: 只有标题，无关闭按钮
- 全屏面板: 标题 + 右侧关闭按钮

**无 Header 时：**

Handle Group 的 `itemSpacing: 12` 仍保留（只有一个子元素时不影响布局），整体结构不变：

```
Top Area (paddingBottom=8)
  └── Column (itemSpacing=16)
        ├── Handle Group (paddingTop=12, itemSpacing=12)
        │     └── Drag Handle (仅此一项)
        └── Content (paddingH=20, paddingV=见下方)
```

> **注意**: 无标题 + Banner 型 Panel（Type 3）的 Content paddingTop 可能为 **8**（而非标准 16），因为 Banner 自带视觉间距。表单型 Content 仍用标准 paddingTop=16。

### 3. Banner（可选）

仅用于促销/功能说明插图（Type 3 常用）。

- 宽度: 面板宽 - 16×2 = 361px（左右各缩 16px）
- 背景: `Bg_Card_App` (#141414)
- 圆角: 16px
- 高度: 通常 204px，可灵活调整

### 4. Content Area（内容区）

**灵活区域 — 可容纳任何内容。**

```
AutoLayout (vertical, w=fill, h=auto, padding=[16, 20, 16, 20], gap=按内容类型)
  └── 任意内容: 文字、表单、列表、卡片...
```

| 内容类型 | itemSpacing |
|---------|-------------|
| 文字段落 | 8 |
| 表单字段 | 24 |
| 列表项 | 8 |
| 信息行 (label-value) | 8 |
| 混合区块 | 16 (默认) |

- **paddingTop/Bottom: 16** — 经 Flutter 源码验证（非 24）
- **paddingLeft/Right: 20** — 与页面标准一致

### 5. Button Area（底部按钮）

**优先使用 Button_Group 组件（内置 iOS Home Bar + padding + Home Bar，一步到位）。**

> **⚠️ 背景色适配：** Button_Group 默认背景是 Bg_Page (#000000)。**Panel 内必须覆盖为 Bg_Float (#0D0D0D)**：
> ```jsonc
> { "type": "instance", "componentKey": "...", "layoutSizingHorizontal": "FILL",
>   "backgroundVariable": "cc4a96ef8296f4752bf28096ab31f71ebc84b096",
>   "background": {"r": 0.051, "g": 0.051, "b": 0.051} }
> ```

| 场景 | 组件 | componentKey | 内置 Home Bar |
|------|------|-------------|:---:|
| 单按钮（推荐） | Button_Group (No.=1) | `ee497740062678d11225cabca6460a60dce92461` | YES |
| 双按钮垂直 | Button_Group (No.=2) | `fb247c6b7c952ed7823f974d138d24a5ab7c2572` | YES |
| 双按钮水平 | Button_Group (2H) | `fe01415f617a6713698adda8b28dbd0179fdf66c` | YES |
| 三按钮 | Button_Group (No.=3) | `ea76f49a7c4d74de3da265a7df3555975ca18dda` | YES |

> **按钮类型决定用 Button_Group 还是手动拼接：**
>
> | 按钮类型 | 方式 | 原因 |
> |---------|------|------|
> | Primary / Secondary | **Button_Group** | 内置 padding + Home Bar，一步到位 |
> | Trading Buy / Sell（单个） | **手动拼接** | Button_Group 内部是 Primary，无法替换为 Trading 样式 |
> | Trading Buy + Sell（并排） | **手动拼接** | 需要双色按钮 |
>
> **Button_Group 用法（Primary/Secondary 场景）：**
> ```jsonc
> { "type": "instance", "componentKey": "ee497740...",
>   "layoutSizingHorizontal": "FILL",
>   "backgroundVariable": "cc4a96ef...", "background": {"r": 0.051, "g": 0.051, "b": 0.051},
>   "overrides": { "Primary": "Confirm" } }
> ```
>
> **手动拼接（Trading Buy/Sell 场景）：**
> ```jsonc
> // Button Area frame (padding: T16 L20 R20 B8)
> //   └── Button_Trading Buy/Sell XL (layoutSizingHorizontal: "FILL")
> // .iOS_Home Bar (layoutSizingHorizontal: "FILL")
> ```
> - Button_Trading Buy XL: `1786d399ac5ccdf07d220122157c5b8da0e39f11`
> - Button_Trading Sell XL: `573da35491e90705195c2988ae6f6fdf0b0e4db2`
> - .iOS_Home Bar: `f98cab636e44f9000be92bbb96cfbf3addb81c8e`
> - Buy+Sell 并排时：两个 Trading 按钮水平排列，各 `layoutSizingHorizontal: "FILL"`，间距 8

### 6. iOS Home Bar 去重规则

| 底部组件 | 内置 Home Bar | 需额外 .iOS_Home Bar？ |
|---------|:---:|:---:|
| Button_Group (任意变体) | YES | **NO** |
| 单独 Button_Primary/Secondary/Trading | NO | **YES** |
| 无按钮（纯内容面板） | — | **YES** |

- .iOS_Home Bar componentKey: `f98cab636e44f9000be92bbb96cfbf3addb81c8e`

---

## 高度变体

### 半屏面板（默认）

- Panel 从屏幕约 60% 位置开始（y ≈ 204 on 852px 屏幕）
- **无关闭按钮**，仅 Drag Handle + 标题
- 上方区域: 暗色遮罩 (opacity=0.6, layoutGrow=1)
- 适用: 内容无需滚动

### 全屏面板

- Panel 从状态栏下方开始（y ≈ 57）
- **有关闭按钮**（24×24 X 图标，标题栏右侧）
- 适用: 内容较长需要滚动

**判断规则**: 内容（Header + Content + Button + Home Bar）超过 ~600px → 全屏 + 关闭按钮

---

## 页面级结构

半屏面板作为浮层覆盖在页面上，完整页面结构:

```
Root (393×852, vertical, itemSpacing=0, bg=#000, clipsContent=true)
  ├── Dimmed Background (layoutSizingVertical="FILL", bg=#000, opacity=0.6)
  └── Panel (vertical, h=auto, bg=Bg_Float, cornerRadius=[16,16,0,0])
        ├── Top Area (paddingBottom=8)
        │     └── Column (itemSpacing=16)
        │           ├── DragHandle+Header Group (paddingTop=12, itemSpacing=12)
        │           │     ├── Drag Handle
        │           │     └── Header
        │           └── Content (paddingH=20, paddingV=16)
        └── Button Area + iOS Home Bar
```

- Root `primaryAxisSizingMode: FIXED`, `counterAxisSizingMode: FIXED`
- Dimmed Background 用 `layoutSizingVertical: "FILL"` 填充 Panel 上方剩余空间
- **Panel 用 `layoutSizingVertical: "HUG"`**，高度随内容自适应

---

## 颜色变量绑定

| 元素 | Token | Variable Key | Hex |
|------|-------|-------------|-----|
| 面板背景 | `Gray[Bg]/Bg_Float` | `cc4a96ef8296f4752bf28096ab31f71ebc84b096` | #0D0D0D |
| Banner/卡片背景 | `Gray[Bg]/Bg_Card_App` | `f330b2b81049533f6a1627c057c12b614b41c9d4` | #141414 |
| 嵌套元素 | `Gray[Ele]/Ele_line` | `8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13` | #1E1F24 |
| 标题/主文字 | `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF |
| Drag Handle/次要文字 | `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 |
| CTA 按钮填充 | `Brand/700-normal` | `0d73577f4ec792b83e33db70b6d3929d17925379` | #FF9C2E |

---

## 设计检查清单

1. **Drag Handle 始终为第一个子元素** — 用组件实例 `8ba02c67...`（禁止手绘 rect），不可省略
2. **标题用 Semibold/18** — 不是 16，不是 20。**Header 是可选的**，无标题时 Handle Group 仅含 Drag Handle
3. **Content 上下 padding = 16** — `paddingTop: 16, paddingBottom: 16`（经 Flutter 源码验证）
4. **Content 左右 padding = 20** — 与页面标准一致
5. **底部按钮优先用 Button_Group** — 内置 iOS Home Bar
6. **所有按钮必须撑满父容器** — `layoutSizingHorizontal: "FILL"`
7. **全屏面板有关闭按钮，半屏面板没有**
8. **iOS Home Bar 去重** — Button_Group 已内置，不要重复添加
9. **面板背景 #0D0D0D (Bg_Float)** — 不是 #000000 (Bg_Page)，不是 #141414 (Bg_Card)
10. **仅顶部两角 16px 圆角** — 用 `topLeftRadius: 16, topRightRadius: 16, bottomLeftRadius: 0, bottomRightRadius: 0`，不要用 `cornerRadius: 16`
11. **Panel 高度 HUG** — `layoutSizingVertical: "HUG"`，暗色遮罩用 `layoutSizingVertical: "FILL"`
12. **DragHandle+Header 分组间距 12** — DragHandle 所在分组 paddingTop=12, 内部 spacing=12, 分组到 Content spacing=16
