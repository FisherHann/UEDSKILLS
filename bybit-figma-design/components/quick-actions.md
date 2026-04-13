# Quick Actions (金刚区)

> 金刚区图标网格（复合组件，用 AutoLayout 拼装）
> Updated: 2026-04-03
> **经 Flutter 源码验证的准确规范。**

---

## AutoLayout 拼装规范

### 单个网格项（Action Item）

```
Column (VERTICAL, w=FILL, h=HUG, gap=4, crossAxisAlign=CENTER)
  ├── 图标容器 (VERTICAL auto-layout, 48×48 FIXED, CENTER+CENTER, bg=Ele_line, cornerRadius=24)
  │     └── Icon 实例 (22×22, 居中)
  └── 标签文字 (12px, weight=400, lineHeight=18, T1, center, w=FILL)
```

#### ⚠️ 关键规则

1. **网格项无 cornerRadius 无 clipsContent** — 网格项 frame 不设任何圆角，不设 clipsContent，会裁切内容
2. **图标容器是 auto-layout + FIXED sizing** — `layoutMode: "VERTICAL"`, `width: 48, height: 48`, `primaryAxisSizingMode: "FIXED"`, `counterAxisSizingMode: "FIXED"`, `primaryAxisAlignItems: "CENTER"`, `counterAxisAlignItems: "CENTER"`。**FIXED sizing 保持 48×48 不变，CENTER 让 icon 居中。绝不用 padding 实现，绝不用 HUG（会丢失固定尺寸）**
3. **icon 尺寸固定 22×22** — 不论 icon_ 还是 BL_ 图标，统一 22×22。居中于 48×48 容器内

### 整行容器

```
Row (HORIZONTAL, w=FILL, h=HUG, gap=8, counterAxisAlign=MIN)
  ├── 网格项 ×N (每项 layoutSizingHorizontal="FILL"，平均分布)
```

---

## JSON 代码模式（直接复制）

### 标准模板（icon 22×22，不分类型）

```jsonc
{
  "type": "frame",
  "name": "Quick Actions",
  "layoutMode": "HORIZONTAL",
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "itemSpacing": 8,
  "counterAxisAlignItems": "MIN",
  "children": [
    {
      "type": "frame",
      "name": "Transfer",
      "layoutMode": "VERTICAL",
      "layoutSizingHorizontal": "FILL",
      "layoutSizingVertical": "HUG",
      "itemSpacing": 4,
      "counterAxisAlignItems": "CENTER",
      // ⚠️ 不设 cornerRadius, 不设 clipsContent
      "children": [
        {
          "type": "frame",
          "name": "Icon Container",
          "layoutMode": "VERTICAL",
          "width": 48, "height": 48,
          "primaryAxisSizingMode": "FIXED",
          "counterAxisSizingMode": "FIXED",
          "primaryAxisAlignItems": "CENTER",
          "counterAxisAlignItems": "CENTER",
          "cornerRadius": 24,
          "backgroundVariable": "8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13",
          "background": {"r": 0.12, "g": 0.12, "b": 0.14},
          "children": [
            {
              "type": "instance",
              "componentKey": "ICON_KEY_HERE",
              "name": "icon_name",
              "width": 22, "height": 22,
              "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
              "color": {"r": 1, "g": 1, "b": 1}
            }
          ]
        },
        {
          "type": "text",
          "content": "Transfer",
          "textStyle": "Regular/12",
          "fontSize": 12, "fontWeight": 400, "lineHeight": 18,
          "textAlignHorizontal": "CENTER",
          "layoutSizingHorizontal": "FILL",
          "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
          "color": {"r": 1, "g": 1, "b": 1}
        }
      ]
    }
    // ... 重复 N 项
  ]
}
```

### BL_ 彩色图标

BL_ 图标与普通 icon 结构完全一致，**同样 20×20**，唯一区别是不设 colorVariable（BL_ 自带颜色）：

```jsonc
{
  "type": "frame",
  "name": "Icon Container",
  "layoutMode": "VERTICAL",
  "width": 48, "height": 48,
  "primaryAxisSizingMode": "FIXED",
  "counterAxisSizingMode": "FIXED",
  "primaryAxisAlignItems": "CENTER",
  "counterAxisAlignItems": "CENTER",
  "cornerRadius": 24,
  "backgroundVariable": "8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13",
  "background": {"r": 0.12, "g": 0.12, "b": 0.14},
  "children": [
    {
      "type": "instance",
      "componentKey": "BL_ICON_KEY_HERE",
      "name": "BL_icon_name",
      "width": 22, "height": 22
      // BL_ 图标已有颜色，不设 colorVariable
    }
  ]
}
```

---

## 颜色变量绑定

| 元素 | Token | Variable Key | Hex |
|------|-------|-------------|-----|
| 图标容器背景 | `Gray[Ele]/Ele_line` | `8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13` | #1E1F24 |
| 标签文字 | `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF |

---

## 尺寸参考（Flutter 源码验证）

| 参数 | 值 | 来源 |
|------|-----|------|
| 整行宽度 | 353px (= 393 - 20×2) | 父容器 FILL |
| 项间距 | 8px | Row spacing |
| 单项宽度 | ~82px (FILL 均分) | (353 - 8×3) / 4 = 82.25 |
| 项圆角 | **无** | ⚠️ 绝不设 |
| 项 clipsContent | **无** | ⚠️ 绝不设 |
| 图标容器 | 48×48 auto-layout FIXED | VERTICAL + FIXED sizing + CENTER 居中 |
| 图标容器圆角 | 24px | 圆形 |
| 图标容器背景 | Ele_line #1E1F24 | |
| icon（所有类型） | **22×22** | 居中偏移 13px |
| icon ↔ label 间距 | 4px | Column spacing |
| label 字号 | 12px Regular | Inter 400 |
| label 行高 | 18px (1.5×) | |
| label 颜色 | T1 #FFFFFF | |
| label 对齐 | CENTER | textAlignHorizontal |
| label 宽度 | FILL (匹配 item 宽度) | |

---

## 常见错误

| ❌ 错误 | ✅ 正确 | 原因 |
|---------|---------|------|
| 图标容器不设 layoutMode（plain frame） | 用 auto-layout + FIXED sizing | plain frame 子元素在 (0,0) 不居中 |
| 图标容器用 auto-layout + HUG | 用 auto-layout + **FIXED** sizing | HUG 会丢失 48×48 固定尺寸 |
| 图标容器用 `padding` + HUG | 用 FIXED + CENTER 对齐 | padding 方式创建了错误的 auto-layout 结构 |
| icon 用 20×20 或 32×32 | **固定 22×22** | 所有 icon（含 BL_）统一 22×22，不分类型 |
| 网格项设 `cornerRadius: 32` | 不设 cornerRadius | 会裁切图标和文字 |
| 网格项设 `clipsContent: true` | 不设 clipsContent | 同上 |
| 整行 `counterAxisAlignItems: "CENTER"` | 用 `"MIN"` | 多行文字时 CENTER 导致错位 |
