# Tabs

> 标签页切换（默认 Tab）
> Updated: 2026-04-06
> Source: Figma「App组件场景使用规范-2026」Tab 规范区

---

## ⛔ Tab 层级语义（MANDATORY — 选型前必读）

Tab 分为三个层级（L1 L2 L3），代表信息强度递减。**不是一定要从 L1 开始使用。**

| Level | 语义 | 说明 | 可单独使用？ |
|-------|------|------|-------------|
| **L1** | **页面级** | 管理整个页面的所有内容切换。该 Tab 下的所有区块内容都归属于它 | ✅ |
| **L2** | **主题级** | 同一主题下内容维度切换（数据列表、仓位管理） | ✅ |
| **L3** | **分类级** | 同一内容下信息分类维度切换（行情标签、产品切换） | ⛔ **不能单独出现，必须搭配 L1 或 L2** |

### 尺寸与字号对应

| Level-Size | fontSize | 高度 |
|-----------|----------|------|
| L1-L | 16px | 24px |
| L1-M | 14px | 22px |
| L2-L | 16px | 30px |
| L2-M | 14px | 28px |
| L3-M | 14px | 28px |
| L3-S | 12px | 24px |

---

## ⛔ 多层 Tab 组合规则（MANDATORY — 禁止跳级）

**核心原则：上层尺寸 ＞ 下层尺寸，层级递减，不可跳级。**

### 单层场景

- ✅ L1 单独使用
- ✅ L2 单独使用
- ⛔ **L3 不能单独使用** — 必须有 L1 或 L2 作为上层

### 二层场景（仅以下 3 种合法搭配）

| 上层 | 下层 | 适用场景 | 示意 |
|------|------|---------|------|
| **L1-L** | **L2-M** | 大信息层级区分（行情页） | `HOT HOT ...` → `ALL Trending Metaverse ...` |
| **L2-L** | **L3-M** | 列表内容切换（订单记录、仓位） | `ALL Trending ...` (underline) → `ALL Trending ...` (pill) |
| **L2-M** | **L3-S** | 卡片等较小空间需适度缩小 | `ALL Trending ...` (smaller) → `ALL Trending ...` (pill-S) |

**⛔ 禁止的搭配：**
- ❌ L1 → L3（跳过 L2）
- ❌ Size L → Size S（尺寸跳级）
- ❌ 同层同尺寸（如 L2-M → L2-M，无法区分层级）
- ❌ 下层尺寸 ≥ 上层尺寸（违反递减原则）

### 三层场景

唯一合法搭配：**L1-L → L2-M → L3-S**（严格递减）

---

## Quick Reference — 默认 Tab (Dark Mode)

### Type = L1（页面级标签，最大）

| Status | Size | componentKey |
|--------|------|-------------|
| Active | L | `af31bba292dc163ef4408a005f606853981f344a` |
| Active | M | `4b665361225aac3e99a9b48ccaf4bbccd94b4fe3` |
| Deactive | L | `84216841063d054676c124b4b634ff3ef82ab24c` |
| Deactive | M | `73f76bbcc60d4b7a358466a6937e28eb75947fd0` |

### Type = L2（主题级标签，常用）

| Status | Size | componentKey |
|--------|------|-------------|
| Active | L | `65a0afbbee47135f3be9733b01e8131aff1f5546` |
| Active | M | `d112f31d85d5a2140b17f3f57bde06ccd01c8be6` |
| Deactive | L | `f1751cc9d326c9492258368703cfdbc9ede65382` |
| Deactive | M | `73d6933f0bfe4587bd5db125a0c933f8223236d9` |

### Type = L3（分类级标签，最小，⛔ 不能单独使用）

| Status | Size | componentKey |
|--------|------|-------------|
| Active | M | `a6f7dea8cba697394ead96603600f0b6953636b5` |
| Active | S | `9e93428dbbb707007918012b5570fa8184b3f20c` |
| Deactive | M | `05aed3822cfa31993904295e5b9c69f793e91944` |
| Deactive | S | `7fd0e8ff3d924449f525a17b10b1debb81fab721` |

---

## 组件规格

> Set Key: `adc375796c077233077b4e65f54de76d149b286a`

- **变体维度**: Status (Active / Deactive) × Type (L1 / L2 / L3) × Size (L / M / S)
- **总变体数**: 12
- **注意**: L1/L2 仅 L/M 尺寸；L3 仅 M/S 尺寸

---

## 选型指南

**选型决策树：**

```
1. 这个 Tab 切换的是整个页面的内容？       → L1
2. 这个 Tab 切换的是同一主题下的不同维度？   → L2
3. 这个 Tab 是在已有 Tab 下的更细分类？     → L3（必须有上层 L1/L2）
4. 如果页面同时有两层 Tab，查组合规则表选尺寸
```

| Type | 语义 | 可用 Size |
|------|------|----------|
| **L1** | 页面级 — 所有页面内容归属于此 Tab | L (16px), M (14px) |
| **L2** | 主题级 — 同一主题下内容维度切换 | L (16px), M (14px) |
| **L3** | 分类级 — 信息分类维度，⛔ 不能单独出现 | M (14px), S (12px) |

---

## Tab 行拼装规范

**Tab 组件是单个标签项**，需要用 AutoLayout 水平排列组成完整的 Tab Bar：

```jsonc
{
  "type": "frame",
  "name": "Tab Bar",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 20,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "counterAxisAlignItems": "CENTER",
  "children": [
    {
      "type": "instance",
      "componentKey": "<Active variant key>",
      "name": "Tab Active",
      "overrides": {"Tab": "Tab Label 1"}
    },
    {
      "type": "instance",
      "componentKey": "<Deactive variant key>",
      "name": "Tab Inactive 1",
      "overrides": {"Tab": "Tab Label 2"}
    },
    {
      "type": "instance",
      "componentKey": "<Deactive variant key>",
      "name": "Tab Inactive 2",
      "overrides": {"Tab": "Tab Label 3"}
    }
  ]
}
```

### ⚠️ 关键规则

1. **Tab 间距固定 20px** — `itemSpacing: 20`，不要用 16 或 24
2. **同一行的 Tab 必须用同一 Type 和 Size** — 不要混用 L1 和 L2
3. **Active 只有一个** — 其余全部用 Deactive 变体
4. **Override key 为 "Tab"** — 用 `"overrides": {"Tab": "实际标签文案"}` 替换默认文案

---

## JSON 示例

### 页面级 Tab Bar（L1 M）— 如 TradFi 产品切换

```jsonc
{
  "type": "frame",
  "name": "Tab Bar",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 20,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "counterAxisAlignItems": "CENTER",
  "children": [
    {
      "type": "instance",
      "componentKey": "4b665361225aac3e99a9b48ccaf4bbccd94b4fe3",
      "name": "Tab Active",
      "overrides": {"Tab": "TradFi CFD"}
    },
    {
      "type": "instance",
      "componentKey": "73f76bbcc60d4b7a358466a6937e28eb75947fd0",
      "name": "Tab xStocks",
      "overrides": {"Tab": "xStocks"}
    },
    {
      "type": "instance",
      "componentKey": "73f76bbcc60d4b7a358466a6937e28eb75947fd0",
      "name": "Tab Perpetuals",
      "overrides": {"Tab": "Perpetuals"}
    }
  ]
}
```

### 区块级 Tab Bar（L2 M）— 如 Order Type 切换

```jsonc
{
  "type": "frame",
  "name": "Order Type Tabs",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 20,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "counterAxisAlignItems": "CENTER",
  "children": [
    {
      "type": "instance",
      "componentKey": "d112f31d85d5a2140b17f3f57bde06ccd01c8be6",
      "name": "Tab Active",
      "overrides": {"Tab": "Limit"}
    },
    {
      "type": "instance",
      "componentKey": "73d6933f0bfe4587bd5db125a0c933f8223236d9",
      "name": "Tab Market",
      "overrides": {"Tab": "Market"}
    },
    {
      "type": "instance",
      "componentKey": "73d6933f0bfe4587bd5db125a0c933f8223236d9",
      "name": "Tab Conditional",
      "overrides": {"Tab": "Conditional"}
    }
  ]
}
```

---

---

## Tab_Trade（Buy/Sell 切换 Tab）

> 内置两个选项的 Trade Tab，用于 Buy/Sell、Long/Short 等二选一场景
> 组件名: `Tab_Average_text`

### Set Key: `ae93499fe813550db72e30507663fe86c540849b`

- **变体维度**: Dark (yes / no) × Type (gray / red / 0.1red / green / 0.1green / yellow / 0.1yellow)
- **总变体数**: 14
- **内置两个选项** — 不需要像默认 Tab 一样手动拼装多个实例

### Quick Reference — Dark Mode

| Type | componentKey | 用途 |
|------|-------------|------|
| gray | `2c22cca6b4cff23fe0c5ea01c1137532f792ce04` | 中性/未选中状态 |
| green | `6034262138fa70975235532feb05c697f13ee6c9` | Buy/Long 选中（实色绿） |
| 0.1green | `e3bda50f8fa25a922faa1784df2b3f3f401e99d8` | Buy/Long 选中（浅绿底） |
| red | `86f4ec6e68ecef666d19728f0469f81c13c7255f` | Sell/Short 选中（实色红） |
| 0.1red | `23b21132b5d8e2c97c3536e8b69cbe42f97a4969` | Sell/Short 选中（浅红底） |
| yellow | `b2cebb4cb3ee3a8ee7caf12a768468893b2c3bd2` | Brand 选中（实色黄） |
| 0.1yellow | `6c2a672e1fbc5de58d88790dac70eb605de5f93a` | Brand 选中（浅黄底） |

### Quick Reference — Light Mode

| Type | componentKey | 用途 |
|------|-------------|------|
| gray | `f1d7bbe9b14c3c7ad3b14a7bc9363adbb601ccd0` | 中性/未选中 |
| green | `e2eb9c6a700e18d3f3c93fd556d08c7cac4be2e1` | Buy/Long 选中 |
| 0.1green | `6fbd747b9fe2b6c170839ff6dd348385f6612782` | Buy/Long 浅底 |
| red | `c898dab33dd801c685107ee1032eb8bb46fc3e41` | Sell/Short 选中 |
| 0.1red | `b98c5f5900a9a8e44ed9bed3d29ad2f426b52c84` | Sell/Short 浅底 |
| yellow | `66995151af9bc3ccb9016b5de17577207b67889b` | Brand 选中 |
| 0.1yellow | `794a96c4e471fbf327b2657bcd02df9ee3bb079b` | Brand 浅底 |

### 选型指南

| 场景 | 推荐 Type |
|------|----------|
| Buy/Sell 切换（Buy 选中） | `green` 或 `0.1green` |
| Buy/Sell 切换（Sell 选中） | `red` 或 `0.1red` |
| 通用二选一（无涨跌语义） | `gray` |
| Brand 强调选中 | `yellow` 或 `0.1yellow` |
| 实色 vs 0.1 浅底 | 实色更强调，0.1 浅底更柔和 |

### JSON 示例

#### Buy/Sell 切换 — Buy 选中（0.1green）

```jsonc
{
  "type": "instance",
  "componentKey": "e3bda50f8fa25a922faa1784df2b3f3f401e99d8",
  "name": "Buy Sell Tab",
  "layoutAlign": "STRETCH"
}
```

#### Buy/Sell 切换 — Sell 选中（0.1red）

```jsonc
{
  "type": "instance",
  "componentKey": "23b21132b5d8e2c97c3536e8b69cbe42f97a4969",
  "name": "Buy Sell Tab",
  "layoutAlign": "STRETCH"
}
```

### ⚠️ 关键规则

1. **不需要手动拼装** — 组件内置两个选项，直接用一个 instance 即可
2. **Type 决定哪个选中** — green 系 = 左侧(Buy)选中，red 系 = 右侧(Sell)选中
3. **0.1 前缀 = 浅底变体** — 视觉更柔和，适合非强调场景
4. **默认用 Dark=yes** — 除非用户指定 Light Mode

---

## 常见错误

| ❌ 错误 | ✅ 正确 | 原因 |
|---------|---------|------|
| 手绘 Tab（frame + text + 下划线 rect） | 用 Tab 组件实例 | 组件带正确的样式和交互态 |
| Tab 间距 24px 或 16px | **固定 20px** | 设计规范统一间距 |
| Active + Deactive 混用不同 Type | 同一行统一 Type/Size | 保持视觉一致 |
| 不 override Tab 文案 | 必须 override 为实际文案 | 避免显示占位符 |
