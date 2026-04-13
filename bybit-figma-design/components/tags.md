# Tags

> 标签：默认标签、营销标签、交易标签
> Updated: 2026-04-01

---

## Quick Reference — 默认 Tag (Dark Mode On, Shape=Default)

通用标签，适用于所有需要 Tag 的场景。来源于 Tag_Trading 组件的 White_Normal / White_Light 变体。

### White_Normal（常规白底标签，最常用）

| Size | componentKey |
|------|-------------|
| S (14h) | `0bc7f9559c858a876440e8697a9510e2d029f332` |
| M (18h) | `dcb8eb42de91417ba37ebd6b46601604260a9e4b` |
| L (22h) | `0dd710332c41fa14f76f6ae0b5bd9d33550ca1c8` |

### White_Light（浅色白底标签）

| Size | componentKey |
|------|-------------|
| S (14h) | `d14ff20ec04f4288df585df54b84dd90230e488d` |
| M (18h) | `3bed02cd7f6d528b11d5d63ef6bee7d82d5cbb69` |
| L (22h) | `e441c9e4bf5729e4c12b3d9ad25d426f214feadd` |

### 默认 Tag — Shape=Leaf (Dark Mode On)

| Type | Size | componentKey |
|------|------|-------------|
| White_Normal | S | `c45b8a35666a3a5498e4396987c58a21ec0ca3ce` |
| White_Normal | M | `50c8e60ec3ace092f5e70fcbe6ee62ae24701946` |
| White_Normal | L | `e62c7cc160541d936cd84f0b8291cc86c416fb5a` |
| White_Light | S | `363e71bafcd470f6af4b380977d6d8a4180a342a` |
| White_Light | M | `3f7aa40bbace1d2c9697352c8687c9a126065c3f` |
| White_Light | L | `e9c44d713736549272904cb8b822d826d5c2dc45` |

---

## Tag_Sales — 营销标签 (Dark yes, Disable=no)

### Gradient（渐变标签）

| Size | componentKey |
|------|-------------|
| S (14h) | `babe0c186003ecb01c01d319ad63d4cddfa69229` |
| M (18h) | `4e6670da54b875e39f99519660bb77434a2fece1` |
| L (22h) | `6cc0a0a81b0a7e70ff6e245ba529fd74a4fc0f18` |

### Gradient-leaf（渐变叶形标签）

| Size | componentKey |
|------|-------------|
| S (14h) | `cc88cded951fe45bdf33132a46264112bb16cb55` |
| M (18h) | `ab872f85b2a76e097b707bb10e81061f5a43c572` |
| L (22h) | `fd86a23618b7c7ec64c1cdbb05a49f78ea002d2c` |

### 其他 Type（仅 S/M 尺寸）

| Type | Size | componentKey |
|------|------|-------------|
| Hot | S (14h) | `6574e43f45c22f2737399f986fe3487c7262fd7f` |
| Hot | M (18h) | `30ec7807f345e8b6a8fc7b936c21d563b6d7482a` |
| Brand | S (14h) | `9db14f386d33a479e76a7ec1e4aa690e04c00539` |
| Text tag | S (14h) | `f262d9f74aeb12da697c1369a4d22716509ece58` |

### Disable 变体 (Dark yes)

| Type | Size | componentKey |
|------|------|-------------|
| Gradient | S | `0cd6ac2cce78c08a31f82512f354b1dffb78b288` |
| Gradient | M | `c0724b33fe4bc8b1772bd04fe3d33b59964b3f26` |
| Gradient | L | `db8484b979dec4515d4e57c9c4012ad81deb1304` |
| Gradient-leaf | S | `a9fef022792cf2f7acd8287322d10a45c1bff044` |
| Gradient-leaf | M | `a498d758dc48c184d891f7110d2c326e55592e35` |
| Gradient-leaf | L | `cdae948ddec46fe0c8744f320397ccc8a006f1f2` |

---

## Tag_Trading — 交易标签 (Dark Mode On, Shape=Default)

| Type | Size | componentKey |
|------|------|-------------|
| + / Long / Success | S (14h) | `ca355fad8bc1950bc9c97a1add7c253473a97154` |
| + / Long / Success | M (18h) | `35738bfd16e7435b66aafeb9fd5c59a49e8adf9f` |
| + / Long / Success | L (22h) | `225391b3bb96bdafc13340206351d4bc46415ac8` |
| - / Short / Error | S (14h) | `8280f7314b655c88b629ecee7d8990d779960543` |
| - / Short / Error | M (18h) | `d43b874b5b5cc1d690ef50011644362ff6f9dbf1` |
| - / Short / Error | L (22h) | `0a15bdd9fa32e2178f510e04483523e6f4c16f94` |

### Shape=Leaf 变体 (Dark Mode On)

| Type | Size | componentKey |
|------|------|-------------|
| + / Long / Success | S | `4378d9a5a2e5ff79908f5fb18b42395163434d83` |
| + / Long / Success | M | `64e804cf59b3f503feef1f494b3961ef86715bfa` |
| + / Long / Success | L | `09abb9daa90e8197de72cc5cf1b06218bb556da0` |
| - / Short / Error | S | `726bc0660c11700c85d68d7e9a616e60a0dcf20d` |
| - / Short / Error | M | `4213d05b25d1c5f88b828d18d95402ed9a4b2c63` |
| - / Short / Error | L | `5f1212665a42ff14e9a8c0b81e647ffb70620272` |

---

## 涨跌幅标签 (Change Up/Down) — AutoLayout 自绘

> **不使用组件**，用 auto-layout frame 自绘。

### 结构

```jsonc
{
  "type": "frame",
  "name": "Change Tag",
  "layoutMode": "HORIZONTAL",
  "width": 72, "height": 28,
  "primaryAxisSizingMode": "FIXED",
  "counterAxisSizingMode": "FIXED",
  "cornerRadius": 14,
  "backgroundVariable": "<Green or Red variable>",
  "background": {"r": ..., "g": ..., "b": ...},
  "counterAxisAlignItems": "CENTER",
  "children": [
    {
      "type": "text",
      "content": "+0.35%",
      "textStyle": "Semibold/12",
      "fontSize": 12, "fontWeight": 600, "lineHeight": 18,
      "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
      "color": {"r": 1, "g": 1, "b": 1}
    }
  ]
}
```

### 颜色选型

| 状态 | backgroundVariable | RGB |
|------|-------------------|-----|
| 涨 (Up) | `a7c82cc6525f151a801391642165a4bd6f3335c1` (Green/700) | `{"r": 0.024, "g": 0.757, "b": 0.404}` |
| 跌 (Down) | `385d72216004f1730c1712f60ca4bb16dd47e235` (Red/700) | `{"r": 0.91, "g": 0.25, "b": 0.39}` |
| 平 (Flat) | `4d26a22c6de1ea23a1b7be969e3caa7ad333189e` (T3) | `{"r": 0.286, "g": 0.298, "b": 0.31}` |

### 规则
- **固定尺寸 72×28** — 不用 HUG，用 FIXED sizing
- 文字包含 +/- 符号（如 `+0.35%`、`-0.08%`、`0.00%`）
- 字体 Semibold/12，白色，居中
- cornerRadius: **14**（全圆角 = height/2 = 28/2）
- 双轴 CENTER 对齐让文字居中

---

## 组件规格

### 1. Tag_Sales

> 营销/促销标签。
> Set Key: `27032610325eda49d7dd48debf8a9e889db9ef8d`

- **变体维度**: Type × Size × Disable (yes / no) × Dark (yes / no)
- **总变体数**: 32
- **Dark Mode**: 属性名 `Dark`，值为小写 **yes/no**
- **注意**: Hot 仅 S/M，Brand 和 Text tag 仅 S；仅 Gradient/Gradient-leaf 有 Disable 态

### 2. Tag_Trading

> 交易涨跌标签 + 通用默认标签（White_Normal / White_Light）。
> Set Key: `c1e9da3488ae10f94369f19fae55b424e267b03f`

- **变体维度**: Type (4种) × Size (S/M/L) × Dark Mode (On / Off) × Shape (Default / Leaf)
- **总变体数**: 48
- **Dark Mode**: 属性名 `Dark Mode`，值为 **On/Off**

**选型指南**:
- **需要通用 Tag** → 用 White_Normal（常规）或 White_Light（浅色），这是默认标签样式
- **营销/促销场景** → 用 Tag_Sales（Gradient / Hot / Brand）
- **交易涨跌场景** → 用 Tag_Trading 的 + / Long / Success（绿）或 - / Short / Error（红）
- **Size**: S=行内紧凑，M=常规，L=强调展示
- **Shape**: Default=圆角矩形，Leaf=叶形（右上/左下圆角）
