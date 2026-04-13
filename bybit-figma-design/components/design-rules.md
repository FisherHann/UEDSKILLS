# Bybit Design Rules

> Canonical design tokens and layout rules for the Bybit App (dark mode).
> Verified against Figma variable definitions and production page specs.
> **Core principle: every value must come from the token system — no arbitrary colors, sizes, or spacing.**

---

## 1. Color System

All colors use Figma's `{r, g, b}` format (0–1 float). Dark mode is the default.
**Every color must pair `colorVariable` (Figma variable key) with `color` (RGB fallback).** See `color-variables.md` for the full key map.

### 1.1 Background Color Hierarchy

Backgrounds follow a strict nesting order — darker containers wrap lighter ones:

```
Level 0  Bg_Page (#000000)        ← page root
  Level 1  Bg_Card (#141414)      ← section containers, cards
    Level 2  Ele_line (#1E1F24)   ← nested cards, interactive surfaces inside cards
```

| Token | Variable Key | RGB | Hex | Nesting Level |
|-------|-------------|-----|-----|---------------|
| `Gray[Bg]/Bg_Page_App` | `f9b47b8b07c15cd8accb2b1eb6fa1cd0303457a0` | `{0, 0, 0}` | #000000 | L0 — page root |
| `static_Black` | `7d48adfccbfde60e7377b5eaf6deb509ad472740` | `{0.071, 0.071, 0.078}` | #121214 | L0 alt — login pages |
| `Gray[Bg]/Bg_Card_App` | `f330b2b81049533f6a1627c057c12b614b41c9d4` | `{0.078, 0.078, 0.078}` | #141414 | L1 — cards on page |
| `Gray[Bg]/Bg_Float` | `cc4a96ef8296f4752bf28096ab31f71ebc84b096` | `{0.051, 0.051, 0.051}` | #0D0D0D | Panel / bottom sheet bg |
| `Gray[Ele]/Ele_line` | `8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13` | `{0.12, 0.12, 0.14}` | #1E1F24 | L2 — elements inside cards |

**Rules:**
- Never place L2 directly on L0 — always wrap in an L1 container first
- Product-specific accent backgrounds (gold, blue) are exceptions — no variable exists, use raw RGB

### 1.2 Text Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF | Primary — titles, values, active tabs, key content |
| `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 | Secondary — labels, descriptions, inactive tabs, placeholders |
| `Gray[T]/T3` | `4d26a22c6de1ea23a1b7be969e3caa7ad333189e` | #494C4F | Tertiary — hints, timestamps, very low emphasis |
| `Gray[T]/T4_Dis` | `b6463cecd10e51192cebc5e5d2eea3fb29ac2d30` | #383B3D | Disabled — unreachable controls |
| `Text/Gray` | — | #999C9E | Mid-emphasis — secondary values, footnotes |

**Decision tree — which text color?**
1. Title, price value, active state, primary content? → **T1**
2. Label, description, inactive tab, placeholder? → **T2**
3. Secondary numeric value (qty, footnote)? → **Text/Gray**
4. Extremely low emphasis (timestamp)? → **T3**
5. Disabled? → **T4**

### 1.3 Brand & Semantic Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Brand/700-normal` | `0d73577f4ec792b83e33db70b6d3929d17925379` | #FF9C2E | CTA fill, active indicators, brand badges, brand links |
| `Brand/900-text` | `db842d9c60d095c8eb615857fd8b2f72cd392b7c` | #F29229 | Brand text on dark bg (subtle) |
| `Green/700-normal` | `a7c82cc6525f151a801391642165a4bd6f3335c1` | #06C167 | Buy/Long, profit, yield %, positive change, success |
| `Red/700-normal` | `385d72216004f1730c1712f60ca4bb16dd47e235` | #E84064 | Sell/Short, loss, expense, negative change, error |
| `static_White` | `bda6981840f965962231e8c565d930e9b0a3a543` | #FFFFFF | Text on colored bg (green/red/orange buttons) |
| `static_Black` | `7d48adfccbfde60e7377b5eaf6deb509ad472740` | #121214 | Text on brand orange bg |

**Green/Red 使用规则 (CRITICAL):**
- Yield 百分比 (APY, APR) → **Green**
- 正收益金额 (+$412, +$12.99 Cashback) → **Green**
- 负支出金额 (-$1,299.00) → **Red**
- 涨跌幅 (+2.5%, -1.3%) → **Green / Red**
- 买入/做多按钮 → **Green 填充**
- 卖出/做空按钮 → **Red 填充**
- 中性金额 (余额、总资产) → **T1 白色** (不用 Green/Red)

### 1.4 Line & Border Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Ele_border` | `3d674302b69c044d756f796491c084fe4eaf7a78` | — | Border, custom separator lines |
| `Line` (manual) | — | #333638 | Divider lines (prefer Divider component) |

**Rule**: Prefer Divider component (`6254d4094b676ce7609ebefa20dcd2e3f3f8f2ad`) over manual `rect` lines.

### 1.5 Icon Color Rules

**Size rule: All icons must not exceed 32x32px — including BL_ (Business Line) icons.**

| Context | Size |
|---------|------|
| Standard UI icon | **24x24** |
| Small / inline icon | **20x20** or **16x16** |
| Large icon (max, including BL_) | **32x32** |
| Micro icon | **12x12** |

> **CRITICAL**: BL_ 金刚区图标虽然视觉上看起来较大，但在 JSON spec 中同样受 32px 上限约束。绝不设为 48/56/64px。

**Color decision tree:**
1. Brand logo? → `Brand/700-normal`
2. Price up/down, profit/loss trend? → `Green/700-normal` / `Red/700-normal`
3. Low-emphasis hint, chevron, "more"? → `Gray[T]/T3`
4. Everything else → `Gray[T]/T1_Title` (white)

**Strict: functional icons (search, wallet, deposit, settings, staking, earn, freeze, send) are ALWAYS T1 white. Never Brand/Green/Red.**

---

## 2. Typography System

Font: **Inter**. `letterSpacing: 0`. Weight: **400 (Regular) or 600 (Semi Bold)** — 700 is deprecated for UI text.

### 2.1 Type Scale

| fontSize | fontWeight | lineHeight | Use for |
|----------|------------|------------|---------|
| 32 | 600 | 40 | Hero display (App special) |
| 24 | 600 | 32 | Page-level titles |
| 20 | 600/400 | 28 | Section titles |
| 18 | 600/400 | 26 | Card titles, sub-section headers |
| 16 | 600/400 | 24 | Emphasized body, key metrics |
| 14 | 600/400 | 22 | Body text, button labels, descriptions |
| 12 | 600/400 | 18 | Small labels, helper text, tags |
| 10 | 600/400 | 14 | Micro text (badges, OB headers) |
| 8 | 600 | 10 | Badge text only |

### 2.2 Asset Header Typography (资产头部字体规范)

> 来源：Flutter 生产代码验证。资产/余额页面的头部区域有专用字体层级。

| 元素 | fontSize | fontWeight | lineHeight | Color | 例子 |
|------|----------|------------|------------|-------|------|
| **主余额（Hero）** | **32** | **600** | 40 | T1 白 | `120.0000` |
| 约等于值 | 14 | 400 | 22 | T2 灰 | `≈ 30,232.53 USD` |
| 子标签（label） | 12 | 400 | 18 | T2 灰 | `Available Balance`, `In Use` |
| 子数值（value） | 14 | 600 | 22 | T1 白 | `32.00`, `100.28` |
| 选择器标签 | 14 | 400 | 22 | T2 灰 | `Equity` + chevron |

**间距规范:**
- 标签 ↔ 数值: **4px** (强关联)
- 主余额 ↔ 约等于值: **4px**
- 统计区块 ↔ 统计区块: **16px**
- 两列统计之间: **8px** (HORIZONTAL gap)

### 2.3 Typography Rules

1. **One hierarchy per view** — max 4–5 levels from the scale
2. **Weight over size** — differentiate by 400 vs 600 before changing size
3. **Active vs inactive** — active: T1 + 600; inactive: T2 + 400
4. **Financial values** — T1 white; positive yield/profit → Green; loss/expense → Red
5. **Never use fontWeight 300 or 800**

---

## 3. Spacing System

Base unit: **4px**. All values are multiples of 4 (exceptions: 1, 2, 6 for dense/compact contexts).

### 3.1 Spacing Scale

| Value | Use for |
|-------|---------|
| 0 | Root children spacing (sections manage own padding) |
| 1 | Dense data rows (orderbook) |
| 2 | Toggle internal gap |
| 4 | Icon-to-text, indicator-to-label |
| 6 | Compact button row gaps |
| 8 | List item gaps, small section gaps, compact card padding |
| 12 | Medium gaps (nav links, icon grid) |
| 16 | Standard content gap, card internal elements |
| 20 | Standard page horizontal padding |
| 24 | Section internal padding, form fields, tab spacing |
| 32 | Section-to-section spacing |
| 48 | Large section vertical padding |

### 3.2 Page Padding (paddingLeft / paddingRight)

**Standard page: 20px. Be consistent within a page — never mix.**

| Page type | L/R padding | T/B padding |
|-----------|-------------|-------------|
| Standard page | **20** | 16–24 |
| Landing / marketing | **20** | 24–48 |
| Trading (dense) | **12** | 8 |
| **Panel (bottom sheet)** | **20** | Content **16** (paddingTop/Bottom 16)；Button Area paddingTop 16 |

### 3.3 Card Padding by Nesting Level

Padding 随嵌套层级递减，避免左右空间累积过深（Page 20 + Parent 20 + Sub 16 = 56px 太挤）。

| 嵌套层级 | 背景色 | Padding | Corner Radius | 例子 |
|----------|--------|---------|---------------|------|
| L1 标准 card（在 page 上） | Bg_Card #141414 | **16** | **16** | Transaction items, Product cards, 大多数场景 |
| L1 大 section card（在 page 上） | Bg_Card #141414 | **20** | **16** | Unified Wallet, Treasury Yield |
| L1 特色 card（hero, virtual card） | Bg_Card / Brand | **24 / 20** | **16** | Virtual Card |
| L2 子 card（在 L1 card 内） | Ele_line #1E1F24 | **12** | **8** | 卡片内嵌套卡, action tiles |
| L2 紧凑子 card（dense） | Ele_line #1E1F24 | **8** | **8** | 紧凑 action button |

**Card Structure (from official guideline):**
```
Screen (393px)
  └── Page Root (Bg_Page #000000)
        └── Card Container (Bg_Card #141414, cornerRadius: 16)
              │  margin: 20px left/right from screen edge
              │  width: 353px (393 - 20×2) on iPhone 14
              │  width: 335px (375 - 20×2) on iPhone SE
              └── Content Area
                    padding: 16px all sides (top/bottom/left/right)
```

**Rules:**
- **L1 card cornerRadius 统一为 16** — 所有直接在 page 上的卡片使用 cornerRadius 16
- Screen to card edge: **20px** (left/right margin = page padding)
- Card to content: **16px** (inner padding all sides)
- Card top/bottom spacing to next element: **16px**
- 子 card padding 必须 < 父 card padding — 层级越深越紧凑
- L2 子 card 最大 padding 12，不要用 16（否则和父 card 相同，无层级感）
- 独立在 page 上的 Ele_line 卡（非嵌套）视为 action tile，padding 12
- **Card 内部间距严格遵循 4 的倍数** — 4, 8, 12, 16, 20, 24px（同整体页面布局规范）

### 3.4 itemSpacing by Context

| Context | itemSpacing |
|---------|-------------|
| Root children | **0** |
| Section-to-section (in content area) | **32** |
| Section title → section content | **16** |
| Card block-to-block (see 3.5) | **16** |
| Form fields (input → input) | **24** |
| List items (transaction rows) | **8** |
| Inline items (icon + text) | **4–8** |
| Tab items | **16–24** |
| Compact buttons (percentage/chip) | **6–8** |
| Dense data (orderbook) | **1** |

### 3.5 Card Internal Spacing Hierarchy (Proximity Principle)

Card 内部的间距遵循**信息亲密度**原则 — 内容越相关，间距越紧：

| 亲密度 | itemSpacing | 场景 | 例子 |
|--------|-------------|------|------|
| **强关联**（同一信息单元） | **2–4** | label ↕ value 上下对 | "Total Balance" ↕ "$12,345" |
| **中关联**（同组不同字段） | **8** | 同一区块内多个信息行 | APY Row ↕ Term Row ↕ Min Amount |
| **弱关联**（不同功能区块） | **16** | card 内不同 section | 标题区 ↕ 数据区 ↕ 操作按钮 |

**实现方式：** Figma auto-layout 每个 frame 只支持一个 itemSpacing，因此用嵌套 frame 实现分层：

```
Card (itemSpacing: 16)               ← 弱关联：区块间 16px
  ├─ Title Block
  ├─ Data Block (itemSpacing: 8)     ← 中关联：字段间 8px
  │     ├─ Field 1 (itemSpacing: 4)  ← 强关联：label+value 4px
  │     │     ├─ "Label"
  │     │     └─ "Value"
  │     └─ Field 2 (itemSpacing: 4)
  │           ├─ "Label"
  │           └─ "Value"
  └─ Action Button
```

**Common mistakes:**
- 不要在 card 内对所有子元素统一用 16px — 强相关的 label+value 会显得割裂
- 不要把 badge/tag 和标题隔 16px — tag 是标题的修饰，用 8px
- 同一组数据的多行信息用 8px 而不是 16px

---

## 4. Corner Radius System

**Core rule: corner radius follows element hierarchy — larger elements get larger radius.**

### 4.1 Radius Scale with Element Mapping

| Radius | Element Level | Examples |
|--------|---------------|----------|
| **0** | Full-width / edge-to-edge | Pages, dividers, full-width banners |
| **1** | Micro indicator | Active tab underline dot |
| **4** | XS — chips, tags | Percentage buttons, mini tags, small badges |
| **6** | S — toggle internals | Toggle segments, small inner buttons |
| **8** | M — standard interactive | Primary buttons, inputs, action buttons, list items, L2 sub-cards |
| **12** | L — large inner containers | Icon backgrounds (32–40px), inner feature blocks |
| **16** | XL — L1 cards & sheets | **All L1 cards on page**, Modal/bottom sheets (top corners only) |
| **full** | Circle | Avatars, circular icon badges (= width / 2) |

### 4.2 Decision Tree — Which Radius?

```
Is it a page-level container or divider?          → 0
Is it a tiny indicator (underline dot)?           → 1
Is it a small chip, tag, percentage button?       → 4
Is it a toggle segment or small inner element?    → 6
Is it a button, input, list item, L2 sub-card?    → 8
Is it an icon background container (32–40px)?     → 12
Is it an L1 card on page, modal, bottom sheet?    → 16
Is it circular (avatar, round icon)?              → full
```

### 4.3 Hierarchy Example (MyBank Page)

```
Page root                     → 0 (no radius)
  Virtual Card (L1 feature)   → 16
  Unified Wallet (L1 section) → 16
    Card Control buttons      → 8  (standard interactive)
  Deposit/Withdraw cards (L1) → 16
  Treasury Yield card (L1)    → 16
    Product sub-cards (L2)    → 8  (sub-card)
  Transaction list items (L1) → 16
    Icon backgrounds (40px)   → 12 (icon container)
```

**Common mistakes:**
- Don't give L1 cards cornerRadius 8 or 12 — L1 cards on page always use **16**
- Don't give small action buttons the same radius (16) as their parent card — use 8
- Don't give list items a chip-level radius (4) — they're standard cards, use 8
- Don't give L2 sub-cards cornerRadius 16 — L2 uses **8** to distinguish from parent
- Icon background containers (32–40px) are an exception: use 12 for the rounded-square look

---

## 5. Layout Rules

### 5.1 Canvas

| Property | Value |
|----------|-------|
| Width | **393** (iPhone 14/15) |
| Height | **852** (single screen) or calculated for scroll |
| Background | `Bg_Page_App` #000000 |
| layoutMode | **VERTICAL** |
| itemSpacing | **0** |
| primaryAxisSizingMode | **FIXED** |
| counterAxisSizingMode | **FIXED** |
| clipsContent | **true** |

### 5.2 Page Structure

```
Root (393 x H, VERTICAL, itemSpacing: 0, clipsContent: true)
  +-- Nav Bar (instance, FILL horizontal) ← includes built-in StatusBar
  +-- Content Area (frame, VERTICAL, FILL both)
  |     padding: 20/20/16/bottom
  |     itemSpacing: 32 (section-to-section)
  |     +-- Section 1 (FILL horizontal, HUG vertical)
  |     +-- Section 2
  |     +-- ...
  +-- Bottom Bar (Tabbar instance or sticky CTA, FILL horizontal)
```

**Rules:**
1. Nav Bar = first child — it already includes StatusBar, do NOT add a separate StatusBar
2. Content area: `layoutSizingHorizontal: "FILL"`, `layoutSizingVertical: "FILL"` (fills remaining height)
3. Bottom bar = last child
4. All root direct children: `layoutSizingHorizontal: "FILL"` (fill parent width)
5. Root `itemSpacing: 0` — sections manage own padding
6. Content area padding L/R = **20** (standard page)
7. All sections/rows inside Content: `layoutSizingHorizontal: "FILL"`, `layoutSizingVertical: "HUG"`

### 5.3 Section Pattern

```jsonc
{
  "type": "frame",
  "name": "Section Name",
  "layoutMode": "VERTICAL",
  "itemSpacing": 16,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "children": [...]
}
```

Sections on page bg (#000) have no background. Sections that need card treatment use `Bg_Card_App` + `cornerRadius: 16`.

### 5.4 Two-Column Layout (Trading)

```jsonc
{
  "type": "frame",
  "name": "Main Area",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 0,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "FILL",
  "paddingLeft": 12, "paddingRight": 12,
  "children": [
    { "name": "Left Column", "width": 175, "layoutSizingVertical": "FILL" },
    { "name": "Right Column", "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "FILL" }
  ]
}
```

---

## 6. Component Heights

| Element | Height |
|---------|--------|
| Status bar (iOS) | **54** |
| Navigation bar | **56** |
| Header (custom) | **56–64** |
| Tabbar (bottom) | **83** (includes safe area) |
| Button XL | **48** |
| Button L | **40** |
| Button S | **28** |
| Input default | **48** |
| Input compact | **40** |
| Toggle container | **40–44** |
| Toggle segment | **36** |
| Tab row | **28–44** |
| Orderbook row | **20** |
| Info row (label-value) | **20–22** |
| Last price row | **32** |
| Percentage button | **24–28** |
| Divider | **0.5** |
| Icon bg container | **32–40** |
| Action button (icon + label) | **auto** (padded) |

---

## 7. Interaction State Colors

| State | Background | Text | Border |
|-------|------------|------|--------|
| Active tab/toggle | Green (buy) or Brand orange | T1 white | none |
| Inactive tab/toggle | Ele_line or transparent | T2 gray | none |
| Selected chip | Brand/700 | static_Black | none |
| Unselected chip | Ele_line | Text/Gray | none |
| Disabled button | T4 bg | T3 text | none |
| Input focused | — | T1 | Brand/700 bottom |
| Input default | — | T2 (placeholder) | Line bottom |

---

## 8. Naming Conventions

| Element | Pattern | Examples |
|---------|---------|----------|
| Page root | Descriptive | `"MyBank Page"`, `"Order Page - BTCUSDT"` |
| Sections | PascalCase purpose | `"Earn TradFi Section"`, `"Recent Activity"` |
| Rows | Purpose + Row | `"APY Row"`, `"Margin Row"` |
| Components | Human-readable | `"Invest Now Btn"`, `"Search Icon"` |
| Data items | Type + number | `"Ask 1"` – `"Ask 7"` |

Name by **content purpose**, not visual layout. Good: `"Trust Section"`. Bad: `"Gray Area"`.

---

## 9. Tag & Label Rules

**All tags/badges must use component library instances — never hand-draw with frame + text + background.**

### 9.1 Tag Component Lookup

| 场景 | Component | Key (Dark) | Override |
|------|-----------|------------|----------|
| 品牌标签 (PRO, VIP, NEW) | `Tag_Sales Brand S` | `e18aeacb3fd68b5419d294c1898826fd4331292a` | `{"Tag": "PRO"}` |
| 功能标签 (FIXED YIELD, HOT) | `Tag_Sales Gradient S/M/L` | `492793314d9a6a39eb6943bd910faefe3693bdc4` | `{"Tag": "FIXED YIELD"}` |
| 热门标签 | `Tag_Sales Hot S/M` | `06ae6090644992134f2e220d7beb2cbdd76fd46c` | `{"Tag": "HOT"}` |
| 中性文字标签 | `Tag_Sales Text tag S` | `5a2b40082c81847c087653e59d3370903ce4cbd2` | `{"Tag": "Label"}` |
| 正向状态 (Safe, Success, +) | `Tag_Trading + S` | `5c41765a322e230f4a697b87ceef855cda929a4c` | `{"Tag": "Safe"}` |
| 负向状态 (Danger, Error, -) | `Tag_Trading - S` | `9aebc7e51d0aa927e0fa99222d30e117a27b1b27` | `{"Tag": "Danger"}` |
| 中性状态 (Normal, Info) | `Tag_Trading White_Normal S` | `2cb95892376d036e67b3f3ac40e359120070d8c8` | `{"Tag": "Info"}` |
| 涨幅百分比 (+0.35%) | AutoLayout 自绘 | Green/700 bg + white text | 见 tags.md "涨跌幅标签" |
| 跌幅百分比 (-0.08%) | AutoLayout 自绘 | Red/700 bg + white text | 见 tags.md "涨跌幅标签" |
| 持平 (0.00%) | AutoLayout 自绘 | T3 bg + white text | 见 tags.md "涨跌幅标签" |

### 9.2 Rules

1. Always override placeholder text — never leave "Tag" or "Label"
2. Tag text: concise, uppercase for English
3. **Never hand-draw badges/tags** — 任何标签类元素（有背景色+文字、圆点+文字、状态指示器等）都必须用 Tag 组件，不要用 frame + rect + text 手绘。常见错误：用绿色圆点 rect + 文字 text 手绘 "Live" 标签 → 应用 `Tag_Trading + S`
4. **涨跌用 AutoLayout 自绘** — 不要用 Unicode ▲/▼ 字符 + 绿/红文本手绘涨跌，用 auto-layout frame（Green/Red bg + Semibold/12 白字 + padding 6/2 + cornerRadius 4），详见 tags.md "涨跌幅标签"
5. **状态标签用 `Tag_Trading`** — Safe/Danger/Success/Error 等状态标签用 Tag_Trading 的对应变体（+/-/White），不要手绘彩色圆角矩形
6. **选择最接近语义的 Tag 变体** — 参考 9.1 表格。品牌/营销 → Tag_Sales，正向状态/实时 → Tag_Trading +，负向 → Tag_Trading -，中性信息 → Tag_Trading White_Normal。当不确定时优先用组件而非手绘

---

## 10. Color Variable Binding Rules

**Every color in JSON must use `colorVariable` + `color` fallback pair.** Plugin binds the Figma variable; falls back to RGB if import fails.

```jsonc
// Text
{"colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51", "color": {"r": 1, "g": 1, "b": 1}}

// Frame background
{"backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}}

// Instance icon tint
{"colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51", "color": {"r": 1, "g": 1, "b": 1}}

// Rect fill
{"colorVariable": "bda6981840f965962231e8c565d930e9b0a3a543", "color": {"r": 1, "g": 1, "b": 1}}
```

**Exceptions (raw RGB allowed):**
- Product-specific accent backgrounds (gold `{0.918, 0.702, 0.031}`, blue `{0.231, 0.51, 0.965}`) — no matching variable exists
- These must be documented in the JSON with a comment-like name (e.g. `"Gold Icon Bg"`)

---

## 11. Do's and Don'ts

### Do
- Bind every color to a Figma variable (`colorVariable` / `backgroundVariable`)
- Follow background nesting: Page → Card → Ele_line
- Use Green for positive values (yield %, profit), Red for negative (loss, expense)
- Follow corner radius hierarchy: 16 for L1 cards, 8 for buttons/L2 sub-cards, 12 for icon containers
- Use 20px page padding consistently
- Use component instances over custom frames
- Use **Tab** component for tab bars — never hand-draw tabs. See `tabs.md` for L1/L2/L3 variants. Tab 间距固定 **20px**
- Active tab = one instance with Active variant; others = Deactive variant
- Use **Tab_Trade** (Tab_Average_text) for Buy/Sell 二选一切换 — 内置两个选项，一个 instance 即可。See `tabs.md`
- Use `layoutSizingHorizontal: "FILL"` for full-width children (replaces legacy `layoutAlign: "STRETCH"`)
- Use `layoutSizingVertical: "FILL"` for fill-remaining-height elements (replaces legacy `layoutGrow: 1`)
- Use `layoutSizingVertical: "HUG"` for auto-height rows/sections
- **Text wrapping** — use `layoutSizingHorizontal: "FILL"` on text nodes (plugin auto-enables wrapping). Do NOT use explicit `width` for text wrapping
- **All buttons must撑满父容器** — CTA 按钮用 `layoutSizingHorizontal: "FILL"`，并排按钮也用 `layoutSizingHorizontal: "FILL"`，不要让按钮用最小宽度
- **底部按钮优先用 Button_Group** — 单按钮用 `Button_Group (No.=1)`，多按钮用 `Button_Group (No.=2/3/2H)`；Button_Group 内置 iOS Home Bar
- **Buy/Sell 底部按钮例外** — Button_Group 内部按钮无法换成 Button_Trading，需要 Buy/Sell（绿/红）按钮时，手动拼接：`Button_Trading Buy XL` + `Button_Trading Sell XL`（水平排列，`layoutGrow: 1` 均分）+ 底部单独放 `.iOS_Home Bar`
- **iOS Home Bar 去重** — Button_Group / Tabbar 已内置 Home Bar，使用它们时不再单独放 `.iOS_Home Bar`；手动拼接 Button_Trading 或无按钮时才需要 `.iOS_Home Bar`

### Don't
- Don't use bare RGB without `colorVariable` (except product-specific accents)
- Don't use Brand orange for yield/profit percentages — use Green
- Don't use T1 white for expense amounts — use Red
- Don't give small buttons the same radius as their parent section
- Don't use arbitrary spacing (15, 17, 23) — snap to the 4px grid
- Don't mix page padding (some sections 20, others 24) on the same page
- Don't use legacy `layoutAlign: "STRETCH"` / `layoutGrow: 1` — use `layoutSizingHorizontal`/`layoutSizingVertical` instead
- Don't use `primaryAxisSizingMode`/`counterAxisSizingMode` on child frames — only use on Root. Use `layoutSizingHorizontal`/`layoutSizingVertical` on children
- Don't use explicit `width` on text nodes for wrapping — use `layoutSizingHorizontal: "FILL"` instead
- Don't add a separate StatusBar instance when using Navigation Bar — Nav Bar already includes StatusBar
- Don't nest more than 4 levels of frames
- Don't hand-draw tags — use Tag_Sales / Tag_Trading / .涨跌 component instances
- Don't use Unicode ▲/▼ + colored text for price changes — use auto-layout 涨跌标签（见 tags.md）
- Don't hand-draw status badges (Safe/Danger) with frame + bg + text — use `Tag_Trading` component
- Don't hand-draw tabs — use `Tab` component instances from `tabs.md`（L1/L2/L3 + Active/Deactive），间距固定 20px
- Don't use L3 Tab alone — L3 必须有 L1 或 L2 作为上层，不能单独出现
- Don't skip Tab levels — 多层 Tab 禁止跳级（L1→L3 ❌）和尺寸跳级（L→S ❌），必须严格递减。合法二层搭配仅 3 种：L1L+L2M / L2L+L3M / L2M+L3S
- Don't hand-draw Buy/Sell segmented controls — use `Tab_Trade` component from `tabs.md`
- Don't color functional icons with Brand/Green/Red — always T1
- Don't duplicate iOS Home Bar — 页面底部已有 Button_Group 或 Tabbar 时，不要再放 `.iOS_Home Bar`

---

## 12. Card Design Rules (Official Guideline)

> 来源：Figma「卡片-规范区」(App组件场景使用规范-2026)

### 12.1 设计原则

1. **内容容器** — 卡片是信息的容器，将相关内容组合在独立单元内
2. **层次清晰** — 通过边框和间距，与背景形成清晰层级，引导用户视觉焦点
3. **一致性** — 所有卡片的圆角、内边距等样式必须统一，形成家族化特征
4. **灵活性** — 卡片布局应能适应不同类型和数量的内容（文本、图片、数据、按钮）

### 12.2 Card Structure (标准卡片结构)

```
Screen (393px width)
  └── Page Bg (#000000, Bg_Page)
        ├── 20px margin left
        ├── Card Container ─────────────────── 353px wide
        │     background: Bg_Card_App (#141414)
        │     cornerRadius: 16
        │     ├── 16px padding top
        │     ├── 16px padding left
        │     ├── Content Area (321px wide) ── adaptive height
        │     ├── 16px padding right
        │     └── 16px padding bottom
        └── 20px margin right
```

**关键尺寸:**

| 属性 | 393屏幕 | 375屏幕 | 说明 |
|------|---------|---------|------|
| Screen to Card (L/R) | 20px | 20px | = page paddingLeft/Right |
| Card width | **353** | **335** | screen - 20×2 |
| Card cornerRadius | **16** | **16** | 所有 L1 卡片统一 |
| Card inner padding | **16** all sides | **16** all sides | top/bottom/left/right |
| Content area width | **321** | **303** | card - 16×2 |

### 12.3 Card Internal Spacing (内部间距)

**严格遵循 4 的倍数**，按信息紧密程度选择：

| Spacing | 紧密度 | 用途 |
|---------|--------|------|
| **4px** | 极紧密 | icon 与 label、inline badge |
| **8px** | 紧密 | 同组字段行间距 |
| **12px** | 中等 | 2列卡片间距（compact） |
| **16px** | 标准 | 不同区块间距、卡片与卡片间距、card-to-card gap |
| **24px** | 宽松 | 大区块间距、feature section 分隔 |

### 12.4 Grid Card Layouts (栅格化卡片)

卡片内部可按栅格排列子卡片/模块：

```
Layout 1: 单列全宽
┌──────────────────────┐
│ Full-width Card      │
└──────────────────────┘

Layout 2: 双列等分 (gap 12-16px)
┌──────────┐ ┌──────────┐
│ Half     │ │ Half     │
└──────────┘ └──────────┘

Layout 3: 三列等分 (gap 16px)
┌──────┐ ┌──────┐ ┌──────┐
│ 1/3  │ │ 1/3  │ │ 1/3  │
└──────┘ └──────┘ └──────┘

Layout 4: 横向滑动 (固定宽度, 超出屏幕可滑动)
┌──────┐ ┌──────┐ ┌──────┐ ┌──
│ 148  │ │ 148  │ │ 148  │ │ ...
└──────┘ └──────┘ └──────┘ └──
```

**JSON 实现 — 双列等分:**
```jsonc
{
  "type": "frame",
  "name": "Two Column Row",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 12,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "children": [
    { "type": "frame", "name": "Card Left", "layoutSizingHorizontal": "FILL", "height": 100, "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}, "cornerRadius": 16 },
    { "type": "frame", "name": "Card Right", "layoutSizingHorizontal": "FILL", "height": 100, "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}, "cornerRadius": 16 }
  ]
}
```

**JSON 实现 — 三列等分:**
```jsonc
{
  "type": "frame",
  "name": "Three Column Row",
  "layoutMode": "HORIZONTAL",
  "itemSpacing": 16,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  "children": [
    { "type": "frame", "name": "Card 1", "layoutSizingHorizontal": "FILL", "height": 100, "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}, "cornerRadius": 16 },
    { "type": "frame", "name": "Card 2", "layoutSizingHorizontal": "FILL", "height": 100, "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}, "cornerRadius": 16 },
    { "type": "frame", "name": "Card 3", "layoutSizingHorizontal": "FILL", "height": 100, "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4", "background": {"r": 0.078, "g": 0.078, "b": 0.078}, "cornerRadius": 16 }
  ]
}
```

### 12.5 Card Design Do's and Don'ts

**Do:**
- 所有 L1 卡片使用 `cornerRadius: 16` + `Bg_Card_App` (#141414)
- 内部间距严格遵循 4 的倍数
- 用 Divider 分割同一卡片内的不同区域，减少颜色层级
- 信息分区整合 — 相关信息放同一卡片，通过间距体现亲密度
- 卡片自适应宽度 — 用 `layoutSizingHorizontal: "FILL"` 而非固定宽度
- 列表类信息直接排列，不需要每行一个卡片

**Don't:**
- ❌ **卡片层级过多** — 不要在卡片内再嵌套彩色卡片造成 3+ 层颜色嵌套。正确做法：降低模块与卡片颜色对比，用 Divider 分割
- ❌ **高度空间浪费** — 不要给每个列表项单独套一个卡片容器。正确做法：列表项直接排列在同一卡片/页面内
- ❌ **信息过于复杂** — 单张卡片不要塞入过多信息类型。保持信息简洁，必要时拆分
- ❌ **排版拥挤凌乱** — 通过信息整合、合理分区避免过于密集的排版。把握好留白节奏

---

## 13. Asset Header Pattern (资产头部 — 不用 Card)

> 来源：Flutter 生产代码。适用于所有展示资产余额的页面头部（Earn、Assets、MyBank 等）。

### 13.1 核心规则

**资产/余额头部区域直接放在 Bg_Page 上，不包裹 Card。** 这是 Bybit App 的标准模式 — 主余额数字作为页面视觉焦点，直接展示在黑色背景上，不需要 Bg_Card 容器。

### 13.2 何时直接展示 vs 包裹 Card

| 场景 | 处理方式 | 原因 |
|------|---------|------|
| 页面头部的主资产/余额 | **直接放在页面背景上** | 作为页面视觉焦点，无需额外层级 |
| 页面头部的统计数据（Available Balance, In Use 等） | **直接放在页面背景上** | 与主余额同属一个信息区块 |
| 中间内容区的产品卡、功能区 | **包裹 Card (Bg_Card)** | 需要容器来组织独立内容 |
| 列表项、交易行 | **直接排列或包裹 Card** | 视上下文而定 |

### 13.3 Asset Header 结构

```
Content Area (on Bg_Page #000, padding 20 L/R)
  └── Asset Header Section (VERTICAL, gap 16, NO background, NO cornerRadius)
        ├── Equity Label Row (HORIZONTAL, gap 4)
        │     ├── "Equity" (14px/400/T2) + chevron icon (14×14, T2)
        │     └── 或 "Total Equity (USD)" (12px/400/T2) + eye icon
        ├── Balance Block (VERTICAL, gap 4)
        │     ├── "120.0000" (32px/600/T1) ← Hero 余额
        │     └── "≈ 30,232.53 USD" (14px/400/T2) ← 约等于值
        └── Stats Grid (HORIZONTAL, gap 8)
              ├── Column 1 (VERTICAL, gap 4, FILL)
              │     ├── "Available Balance" (12px/400/T2)
              │     └── "32.00" (14px/600/T1)
              └── Column 2 (VERTICAL, gap 4, FILL)
                    ├── "In Use" (12px/400/T2) + info icon
                    └── "100.28" (14px/600/T1)
```

### 13.4 JSON 代码模式

```jsonc
{
  "type": "frame",
  "name": "Asset Header",
  "layoutMode": "VERTICAL",
  "itemSpacing": 16,
  "layoutSizingHorizontal": "FILL",
  "layoutSizingVertical": "HUG",
  // ⚠️ 无 background, 无 cornerRadius — 直接在页面背景上
  "children": [
    {
      "type": "frame",
      "name": "Equity Label",
      "layoutMode": "HORIZONTAL",
      "itemSpacing": 4,
      "counterAxisAlignItems": "CENTER",
      "layoutSizingVertical": "HUG",
      "children": [
        {
          "type": "text", "content": "Equity",
          "textStyle": "Regular/14", "fontSize": 14, "fontWeight": 400, "lineHeight": 22,
          "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
          "color": {"r": 0.416, "g": 0.431, "b": 0.451}
        },
        {
          "type": "instance",
          "componentKey": "7257262c737c3196d0092d97e96796e8d3fa2dd7",
          "name": "icon_chevron_down", "width": 14, "height": 14,
          "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
          "color": {"r": 0.416, "g": 0.431, "b": 0.451}
        }
      ]
    },
    {
      "type": "frame",
      "name": "Balance Block",
      "layoutMode": "VERTICAL",
      "itemSpacing": 4,
      "layoutSizingHorizontal": "FILL",
      "layoutSizingVertical": "HUG",
      "children": [
        {
          "type": "text", "content": "120.0000",
          "fontSize": 32, "fontWeight": 600, "lineHeight": 40,
          "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
          "color": {"r": 1, "g": 1, "b": 1}
        },
        {
          "type": "text", "content": "≈ 30,232.53 USD",
          "textStyle": "Regular/14", "fontSize": 14, "fontWeight": 400, "lineHeight": 22,
          "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
          "color": {"r": 0.416, "g": 0.431, "b": 0.451}
        }
      ]
    },
    {
      "type": "frame",
      "name": "Stats Grid",
      "layoutMode": "HORIZONTAL",
      "itemSpacing": 8,
      "layoutSizingHorizontal": "FILL",
      "layoutSizingVertical": "HUG",
      "children": [
        {
          "type": "frame", "name": "Available Balance",
          "layoutMode": "VERTICAL", "itemSpacing": 4,
          "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "HUG",
          "children": [
            { "type": "text", "content": "Available Balance",
              "textStyle": "Regular/12", "fontSize": 12, "fontWeight": 400, "lineHeight": 18,
              "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
              "color": {"r": 0.416, "g": 0.431, "b": 0.451} },
            { "type": "text", "content": "32.00",
              "textStyle": "Semibold/14", "fontSize": 14, "fontWeight": 600, "lineHeight": 22,
              "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
              "color": {"r": 1, "g": 1, "b": 1} }
          ]
        },
        {
          "type": "frame", "name": "In Use",
          "layoutMode": "VERTICAL", "itemSpacing": 4,
          "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "HUG",
          "children": [
            { "type": "text", "content": "In Use",
              "textStyle": "Regular/12", "fontSize": 12, "fontWeight": 400, "lineHeight": 18,
              "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
              "color": {"r": 0.416, "g": 0.431, "b": 0.451} },
            { "type": "text", "content": "100.28",
              "textStyle": "Semibold/14", "fontSize": 14, "fontWeight": 600, "lineHeight": 22,
              "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
              "color": {"r": 1, "g": 1, "b": 1} }
          ]
        }
      ]
    }
  ]
}
```

### 13.5 常见错误

| ❌ 错误 | ✅ 正确 | 原因 |
|---------|---------|------|
| 把资产余额包在 Bg_Card 卡片内 | 直接放在 Bg_Page 上 | 余额是页面焦点，不需要额外容器 |
| 余额用 24px 字号 | 用 **32px** Hero 字号 | 主余额需要最强视觉权重 |
| 统计子项各自包一个小 Card | 两列平铺，无背景 | 减少层级嵌套，保持简洁 |
| 统计行之间用 16px 间距 | label↔value 用 **4px**，区块间用 **16px** | 信息亲密度原则 |
| 约等于值用 T1 白色 | 用 **T2 灰色** | 次要信息降低视觉权重 |
