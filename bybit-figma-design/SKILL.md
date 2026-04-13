---
name: bybit-figma-design
description: |
  Generate Bybit App design pages — either directly in Figma via Plugin API, or as JSON specs for the Bybit Layout Builder plugin.

  Use this skill when the user asks to create Bybit App designs, mockups, or page layouts.
  Two output modes: Direct Figma (when user provides a Figma URL) or JSON (otherwise).
---

# Bybit Figma Design — Page Generator

Generate Bybit App design pages using real Bybit component library instances. Supports two output modes based on user input.

## Design Philosophy (核心理念)

> **参考图是参考，不是目标。**

用户提供的参考图（截图、竞品、草稿）仅用于理解：
- **页面结构** — 有哪些区块，如何排列
- **内容类型** — 标题、列表、表单、按钮等信息层级
- **交互模式** — Tab 切换、展开折叠、筛选排序等 UX 模式
- **业务语义** — 信息在表达什么（余额、交易对、设置项等）

**你的任务是：理解参考图的结构和意图，然后严格使用 Bybit 设计规范（颜色、字体、间距、圆角、组件库）重新构建为符合 Bybit 设计语言的原生设计稿。**

### 不要做
- ❌ 1:1 像素还原参考图的颜色、间距、字体
- ❌ 照搬参考图的非标准布局（如非 4px 倍数的间距）
- ❌ 因为参考图用了某种自定义样式就手绘，放弃 Bybit 组件库
- ❌ 复制参考图中不符合 Bybit 规范的设计决策

### 要做
- ✅ 从参考图提取信息架构和 UX 模式
- ✅ 用 `design-rules.md` 的 token 系统（颜色、字体、间距、圆角）输出
- ✅ 优先使用 Bybit 组件库（Button、Input、Tag、Switch 等）
- ✅ 遵循 Bybit 的卡片规范（cornerRadius 16、padding 16、page margin 20）
- ✅ 当参考图风格与 Bybit 规范冲突时，**以 Bybit 规范为准**

---

## Output Mode Detection

| User Input | Mode | Output |
|-----------|------|--------|
| Provides a Figma URL (e.g. `figma.com/design/xxx?node-id=...`) | **Mode A: Direct Figma Write** | Builds the page directly in the Figma file via Plugin API (`use_figma`) |
| No Figma URL | **Mode B: JSON Export** | Generates JSON spec → saves to `~/Desktop/bybit-figma-designs/` → copies to clipboard for plugin import |

**⛔ 两个模式严格遵守完全相同的设计规范。** Mandatory Rules、组件选型、颜色 token、间距/圆角/字体规则、Quality Gate 校验 — 一条都不能省。Mode A 不能因为「直接写 Figma 更灵活」就跳过规范检查；Mode B 不能因为「只是 JSON」就放松标准。**输出渠道不同，设计标准完全一致。**

---

## Workflow

### Shared Steps (both modes)

1. **理解参考图意图** — 分析参考图的页面结构、信息层级、内容类型、交互模式。提取「要做什么」而非「长什么样」。
2. **Read design-rules.md** — Always read `components/design-rules.md` before generating to ensure colors, spacing, and typography comply with the spec. Default to Dark Mode.
3. **规范化设计** — 将参考图的结构映射到 Bybit 设计规范：选择正确的颜色 token、字体层级、间距值、圆角半径、组件变体。
4. **组件清单（Bill of Materials）** — 列出页面中**所有需要的 UI 元素**，逐一判断：用库组件还是 custom frame。按 Rule 16 映射表强制匹配。输出格式：
    ```
    - Nav Bar → instance (navigation.md)
    - Hero Title → text (custom)
    - Borrow Button → instance: Button_Primary XL (buttons.md)
    - Security Feature Icon → instance: icon_safety (icons.md)
    - ...
    ```
5. **🔑 查表取 Key（Mandatory Lookup — BLOCKING STEP）** — 对清单中**每一个 instance**，打开对应的组件文件（`buttons.md`、`icons.md`、`tags.md`、`controls.md` 等），**Read 文件并复制 componentKey**。同时查取该组件的 override key（见 Override Rules）。输出 Key 查取结果表：
    ```
    | Component | Source File | componentKey | Override Keys |
    |-----------|-------------|-------------|---------------|
    | Button_Primary XL | buttons.md:14 | 6f623a2d...635 | "Primary" → 按钮文案 |
    | icon_safety | icons.md:405 | d03c0232...725 | (icon, no override) |
    | Tag_Sales Brand S | tags.md:65 | 9db14f38...539 | "Tag"+"Label" → 实际文案 |
    ```
    **⛔ 此步骤为 BLOCKING — 禁止跳过。** 没有产出 Key 查取表就开始写 = 违规。Common Components 表里已有的 Key 可以直接引用（它们已经过验证），但其他所有 Key 必须从源文件查取。

### Mode A: Direct Figma Write (有 Figma URL)

**前置条件**: 用户提供了 Figma 文件 URL，从中提取 `fileKey` 和目标 `nodeId`（页面或画板）。

**⚠️ 必须先加载 `figma-use` skill** — 每次调用 `use_figma` 前必须先 invoke `figma:figma-use` skill。

6A. **解析 URL** — 从 Figma URL 提取 `fileKey` 和 `nodeId`。URL 格式: `figma.com/design/:fileKey/:fileName?node-id=:nodeId`（nodeId 中 `-` 转 `:`）。
7A. **分步构建（Incremental Build）** — 将页面拆分为 3-5 个 `use_figma` 调用，每步返回创建的 node IDs。**绝不在一个脚本里构建整个页面。**

    推荐分步顺序：
    ```
    Step 1: 创建 Root frame + 导入 StatusBar/NavBar + Content frame + BottomBar
           → return { rootId, contentId, navId, ... }
    Step 2: 构建 Header/Hero 区域（资产头部、标题等）
           → return { headerIds: [...] }
    Step 3: 构建产品卡片/列表区域
           → return { cardIds: [...] }
    Step 4: 构建剩余区块（CTA、Footer 等）
           → return { sectionIds: [...] }
    Step 5: 验证 + 修复（用 get_screenshot 检查视觉效果）
    ```

    **⚠️ 每一步都必须使用 Standard Helpers（见下方）在创建节点时同步绑定变量。不要留到最后统一扫描。**

8A. **变量绑定策略 — 边创建边绑定（Inline Binding）**

    **⛔ 禁止「最后统一扫描匹配 RGB」的方式。** 用 RGB 浮点数近似匹配（如 `Math.abs(r-0.416) < 0.02`）绑定变量非常脆弱，容易漏绑或误绑。

    **正确做法：在每个 step 中创建节点时，立即调用 helper 函数绑定变量。** 每个 `use_figma` 脚本开头粘贴 Standard Helpers block（见下方），在创建 frame/text 后紧跟绑定调用：

    ```js
    // 创建卡片 → 立即绑定背景变量
    const card = figma.createFrame();
    card.fills = [{ type: 'SOLID', color: {r:0.078,g:0.078,b:0.078} }];
    parent.appendChild(card);
    await bindBg(card, 'bgCard');  // ← 创建后立即绑定

    // 创建文字 → 立即绑定文字色变量
    const title = figma.createText();
    title.fills = [{ type: 'SOLID', color: {r:1,g:1,b:1} }];
    await bindText(title, 't1');   // ← 创建后立即绑定
    ```

    这样每个节点在创建时就完成了变量绑定，不需要最后再扫描。

9A. **⛔ Quality Gate（与 Mode B 完全一致的 11 项检查）** — 构建完成后，调用 `get_screenshot` 检查视觉效果，逐条校验以下清单：

    ```
    ✅/❌ [Rule 3]  StatusBar — 页面顶部有 StatusBar（Nav Bar 内置或独立实例）？
    ✅/❌ [Rule 8]  背景色 — 所有 frame/card 背景仅使用白名单 token（Bg_Page/Bg_Card/Bg_Float/Ele_line）？无自定义 RGB？
    ✅/❌ [Rule 14] Key 来源 — 每个 componentKey 来自 Common Components 表或步骤 5 查取表？无凭记忆填写？
    ✅/❌ [Rule 15] Asset Header — 如有资产/余额头部，是否直接放页面背景上（无卡片包裹）？
    ✅/❌ [Rule 17] Tag Override — 所有 Tag 的 override 值是实际文案，不是 "Label"/"Tag" 占位符？
    ✅/❌ [Rule 2]  Button FILL — 所有按钮 layoutSizingHorizontal 为 "FILL"（行内小按钮除外）？
    ✅/❌ [Rule 9]  Icon 来源 — 所有图标用 componentKey 引用组件库？无 emoji？无自绘？
    ✅/❌ [Rule 11] Icon 尺寸 — 所有图标 ≤ 32px？
    ✅/❌ [Rule 5]  英文输出 — 所有用户可见文案为英文？
    ✅/❌ [变量绑定] — 所有背景色、文字色通过 Standard Helpers 在创建时绑定了 Figma 变量？
    ✅/❌ [Rule 21] Tab 层级 — 如有 Tab：L3 未单独出现？多层搭配严格递减且无跳级？
    ✅/❌ [100px]   无残留 — 每个 step 的 return 前都调用了 fix100px(root)？截图中无异常 100px 空白？
    ```

### Mode B: JSON Export (无 Figma URL)

6B. **生成 JSON** — **仅使用步骤 5 查取表中的 Key** 编写 JSON spec。写 JSON 时逐个从表中复制 Key，不凭记忆填写。
7B. **⛔ Quality Gate（输出前强制校验 — BLOCKING STEP）** — JSON 写完后、保存文件前，**必须逐条过一遍以下检查清单并输出校验结果**。任何一项 FAIL 必须修复后才能保存。

    ```
    ✅/❌ [Rule 3]  StatusBar — 页面第一个 child 是 StatusBar 实例或含内置 StatusBar 的 Nav Bar？
    ✅/❌ [Rule 8]  背景色 — 所有 frame/card 的 background 仅使用白名单 token（Bg_Page/Bg_Card/Bg_Float/Ele_line）？无自定义 RGB？
    ✅/❌ [Rule 14] Key 来源 — 每个 componentKey 都来自 Common Components 表或步骤 5 查取表？无凭记忆填写？
    ✅/❌ [Rule 15] Asset Header — 如有资产/余额头部，是否直接放页面背景上（无卡片包裹）？
    ✅/❌ [Rule 17] Tag Override — 所有 Tag 的 override 值是实际文案，不是 "Label"/"Tag" 占位符？涨跌标签 cornerRadius = height/2？
    ✅/❌ [Rule 2]  Button FILL — 所有按钮 layoutSizingHorizontal 为 "FILL"（行内小按钮除外）？
    ✅/❌ [Rule 9]  Icon 来源 — 所有图标用 componentKey 引用组件库？无 emoji？无自绘？
    ✅/❌ [Rule 11] Icon 尺寸 — 所有图标 ≤ 32px？
    ✅/❌ [Rule 5]  英文输出 — 所有用户可见文案为英文？
    ✅/❌ [Rule 7]  颜色双写 — 所有颜色同时有 colorVariable + color（或 backgroundVariable + background）？
    ✅/❌ [Rule 21] Tab 层级 — 如有 Tab：L3 未单独出现？多层搭配严格递减且无跳级？仅使用合法组合（L1L+L2M / L2L+L3M / L2M+L3S）？
    ✅/❌ [100px]   无空 frame — 没有 children 为空的 frame 节点？所有间距用 itemSpacing/padding 实现？
    ```

    **没有输出校验结果 = 不能保存 JSON。** 这是流程的一部分，不是可选步骤。

8B. Save to `~/Desktop/bybit-figma-designs/{page-name}.json` and copy to clipboard
9B. User pastes into Figma plugin → Build → design is generated with linked component instances

> **为什么这样设计？** 规则写了但不检查 = 白写。测试中 28 个问题里有 9 个的根因是「没查文件就填 Key」，另有多个是「规则存在但生成时跳过」。Quality Gate 用强制校验清单把「应该遵守」变成「必须验证通过」，确保每次输出都符合规范。

### 🔄 Post-Session Reflection（两种模式通用，Quality Gate 之后）

> **核心原则：发现问题 → 分析根因 → 直接改 SKILL.md。不存日记，不建中间文件。**

**触发条件（满足任一即触发）：**
- 截图发现问题需要二次修复（说明 Helper 或流程有缺陷）
- 用户反馈了 Quality Gate 没覆盖的问题
- `importComponentByKeyAsync` 报 key not found（说明 Common Components 表数据过期）
- `overrideText` 静默失败（说明匹配逻辑有 gap）

**执行步骤：**

1. **列出本次会话中所有需要 > 1 次尝试才修好的问题**，逐个分析根因：

    ```
    | 问题 | 根因分类 | 修什么 |
    |------|---------|--------|
    | Tab 文案没覆盖 | helper-bug | overrideText 匹配逻辑 |
    | USDC key 报错 | stale-data | Common Components 表 |
    | 文案设完截图不变 | api-gotcha | loadFont 返回值 |
    ```

    **根因分类 → 改哪里：**
    | 根因 | 改什么 |
    |------|--------|
    | `helper-bug` | Standard Helpers 代码块 |
    | `stale-data` | Common Components 表 / 组件文件 |
    | `api-gotcha` | Gotchas 表 |
    | `workflow-gap` | Quality Gate 检查项 |
    | `component-misuse` | Mandatory Rules |

2. **直接修改 SKILL.md 对应位置**，用 `📝` 前缀标记演进条目（方便追溯）。

3. **本次无需修复 → 跳过，不做空操作。**

> **Skill 文件就是唯一 source of truth。改它 = 下次生成自动生效。**

---

## Figma Direct Write Reference (Mode A)

> 以下内容仅适用于 Mode A（直接写入 Figma）。Mode B（JSON 导出）跳过此节。

### 前置要求

1. **加载 `figma-use` skill** — 每次会话首次调用 `use_figma` 前，必须先 invoke `figma:figma-use` skill
2. **解析目标页面** — 从 URL 提取 fileKey 和 nodeId，用 `use_figma` 切换到目标页面：
   ```js
   const pages = figma.root.children;
   const targetPage = pages.find(p => p.id === '68:295337'); // nodeId from URL
   if (targetPage) await figma.setCurrentPageAsync(targetPage);
   ```

### Standard Helpers（每个 use_figma 脚本开头粘贴）

**⛔ 每个 `use_figma` 调用开头必须粘贴此 block。** 不要在每个 step 里重新手写这些函数。

```js
// === Standard Helpers — paste at top of every use_figma script ===

// Font loader (handles SemiBold naming) — returns the resolved style name
async function loadFont(family, style) {
  try {
    await figma.loadFontAsync({ family, style });
    return style;
  } catch {
    const alt = style.replace('SemiBold', 'Semi Bold');
    await figma.loadFontAsync({ family, style: alt });
    return alt;
  }
}

// Variable key map
const V = {
  bgPage:  'f9b47b8b07c15cd8accb2b1eb6fa1cd0303457a0',
  bgCard:  'f330b2b81049533f6a1627c057c12b614b41c9d4',
  bgFloat: 'cc4a96ef8296f4752bf28096ab31f71ebc84b096',
  eleLine: '8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13',
  t1:      '06604334971ef5a010e564cf2fddd532e6b52e51',
  t2:      'c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955',
  t3:      '4d26a22c6de1ea23a1b7be969e3caa7ad333189e',
  t4:      'b6463cecd10e51192cebc5e5d2eea3fb29ac2d30',
  brand:   '0d73577f4ec792b83e33db70b6d3929d17925379',
  green:   'a7c82cc6525f151a801391642165a4bd6f3335c1',
  red:     '385d72216004f1730c1712f60ca4bb16dd47e235',
};
const _varCache = {};
async function getVar(key) {
  if (!_varCache[key]) _varCache[key] = await figma.variables.importVariableByKeyAsync(V[key] || key);
  return _varCache[key];
}

// Bind background variable (frame/rect)
async function bindBg(node, varKey) {
  const variable = await getVar(varKey);
  const fills = JSON.parse(JSON.stringify(node.fills));
  if (fills.length === 0) return;
  const newPaint = figma.variables.setBoundVariableForPaint(fills[0], 'color', variable);
  node.fills = [newPaint];
}

// Bind text color variable
async function bindText(textNode, varKey) {
  const variable = await getVar(varKey);
  let fills = JSON.parse(JSON.stringify(textNode.fills));
  if (fills.length === 0) return;
  const newPaint = figma.variables.setBoundVariableForPaint(fills[0], 'color', variable);
  textNode.fills = [newPaint];
}

// Bind icon color (internal vector children)
async function bindIcon(iconInst, varKey) {
  const variable = await getVar(varKey);
  const nodes = iconInst.findAll(n => n.fills?.length > 0 && n.fills[0].type === 'SOLID');
  for (const cn of nodes) {
    const fills = JSON.parse(JSON.stringify(cn.fills));
    const np = figma.variables.setBoundVariableForPaint(fills[0], 'color', variable);
    cn.fills = [np];
  }
}

// Override component text — matches by characters content first, then by node name.
// ⚠️ Setting fontName + characters strips textStyleId — this function saves and restores it automatically.
// 📝 Evolution fix (2026-04-13): Added name-based matching. Tab/Nav components have text nodes
//    named "Tab Name"/"Page_Title" but characters may be "ALL"/"Title" — content-only matching fails.
async function overrideText(instance, searchText, newText) {
  const textNodes = instance.findAll(n => n.type === 'TEXT');
  const s = searchText.toLowerCase();
  // Priority 1: match by characters content (existing behavior)
  let target = textNodes.find(t => t.characters.toLowerCase().includes(s));
  // Priority 2: match by node name (catches Tab Name, Page_Title, etc.)
  if (!target) target = textNodes.find(t => t.name.toLowerCase().includes(s));
  if (!target) return;
  const fn = target.fontName;
  const savedStyleId = target.textStyleId;
  const resolvedStyle = await loadFont(fn.family, fn.style);
  target.fontName = { family: fn.family, style: resolvedStyle };
  target.characters = newText;
  if (savedStyleId && savedStyleId !== figma.mixed) target.textStyleId = savedStyleId;
}

// Text style keys (Typography2025) — use with importStyleByKeyAsync
const TSK = {
  'Semibold/24': '63a28af908557280e4a282f03952ab43729013e5',
  'Semibold/20': '35fb29567075d8502fc1f847e70a5db565d94d47',
  'Semibold/18': 'c263f6c79f9e992bc6d3b1e0762fea8253720a0e',
  'Semibold/16': 'fb6b05c3ebc979b4616801a01aea9b2991a281c7',
  'Semibold/14': '95c43064b7107a35a1c85e28bca7fd870c03dda7',
  'Semibold/12': 'ad093050bf6f479e6c382e0317c64bac1bb428af',
  'Semibold/10': '7f31e3a76bcf8dd27330216dbc054c6602dcbd15',
  'Regular/16':  'eef16a67cb4eba4134ab99e4bdd56bf24c937d5c',
  'Regular/14':  '9e2e5ce326940a39f1a397744f6b126b62c01512',
  'Regular/12':  '8d1cf8fd6e5f75224901553b25d4f1d075e654e7',
  'Regular/10':  'b271c0aacc59f19ad17fe6ec5252470c211ea2b4',
};
// Bind library text style by name (async — must await)
async function bindStyle(textNode, name) {
  if (!TSK[name]) return;
  const style = await figma.importStyleByKeyAsync(TSK[name]);
  if (style) textNode.textStyleId = style.id;
}

// Safe frame creator — prevents 100px default size bug
// Creates frame, appends to parent, sets FILL horizontal. Vertical stays FIXED until children added.
// Call setHug(frame) AFTER all children are appended.
function makeFrame(name, parent, opts = {}) {
  const f = figma.createFrame();
  f.name = name;
  f.layoutMode = opts.direction || 'VERTICAL';
  f.itemSpacing = opts.spacing ?? 0;
  if (opts.padding) {
    f.paddingLeft = f.paddingRight = opts.padding;
    f.paddingTop = f.paddingBottom = opts.padding;
  }
  if (opts.paddingH != null) { f.paddingLeft = f.paddingRight = opts.paddingH; }
  if (opts.paddingV != null) { f.paddingTop = f.paddingBottom = opts.paddingV; }
  f.fills = opts.fills ?? [];
  if (opts.cornerRadius) f.cornerRadius = opts.cornerRadius;
  if (opts.clipsContent) f.clipsContent = true;
  parent.appendChild(f);
  f.layoutSizingHorizontal = opts.hSizing || 'FILL';
  // ⛔ Do NOT set layoutSizingVertical here — wait until children are added
  return f;
}
// Call after all children are appended to set HUG
function setHug(frame) { frame.layoutSizingVertical = 'HUG'; }

// 100px cleanup — call before every return statement
function fix100px(root) {
  let fixed = 0;
  root.findAll(n => n.type === 'FRAME').forEach(f => {
    if (f.children.length === 0 && (f.height === 100 || f.width === 100)) {
      f.remove(); fixed++;
    }
  });
  return fixed;
}

// === End Standard Helpers ===
```

**使用方式：** 每个 `use_figma` 脚本开头粘贴整个 block，然后在创建节点时直接调用：
- `await bindBg(frame, 'bgCard')` — 绑定背景色
- `await bindText(text, 't1')` — 绑定文字色
- `await bindIcon(icon, 'brand')` — 绑定图标色
- `await overrideText(btn, 'Primary', 'Get Started')` — 替换组件文案
- `await bindStyle(text, 'Semibold/20')` — 绑定文字样式（async，必须 await）
- `await getVar('bgCard')` — 获取已缓存的变量对象
- `const section = makeFrame('Section', parent, { spacing: 16, padding: 20 })` — 安全创建 frame（不会触发 100px）
- `setHug(section)` — 所有 children 加完后再调用，设置 HUG
- `fix100px(root)` — **每个脚本 return 前必须调用**，兜底清除残留 100px 空 frame

---

### 核心 API 模式

> **所有函数实现见上方 Standard Helpers block。** 以下仅列关键用法和注意事项。

#### 导入组件并创建实例

```js
const comp = await figma.importComponentByKeyAsync('6f623a2d962d0e04658039c1bbaa20a66379b635');
const btn = comp.createInstance();
btn.name = 'CTA Button';
parentFrame.appendChild(btn);
btn.layoutSizingHorizontal = 'FILL'; // MUST be after appendChild
```

**⚠️ `layoutSizingHorizontal = 'FILL'` 必须在 `appendChild` 之后设置**，否则会报错。

#### 组件文案替换（Override Text）

**关键坑：必须先设 `fontName` 再设 `characters`，否则文案不会视觉更新。** Standard Helpers 中的 `overrideText` 已处理此问题。

```js
// 使用 Standard Helpers
await overrideText(navBar, 'Page_Title', 'Crypto Lending');
await overrideText(button, 'Primary', 'Borrow Now');
```

#### 颜色变量绑定（Variable Binding）

**所有背景色和文字色都必须绑定 Figma 变量，而非只设 RGB。** 使用 Standard Helpers 的 `bindBg` / `bindText` / `bindIcon`，通过 `V` 变量 key map 的别名调用：

```js
await bindBg(card, 'bgCard');     // 背景色绑定
await bindText(title, 't1');      // 文字色绑定
await bindIcon(icon, 'brand');    // 图标色绑定（自动查找内部 vector 子节点）
```

也可传完整 variable key（不在 `V` map 中的变量）：
```js
await bindBg(node, 'f330b2b81049533f6a1627c057c12b614b41c9d4');
```

#### 文字样式绑定（Text Style Linking）

**使用库文字样式绑定，而非只设 fontSize/fontWeight。** Standard Helpers 的 `TSK` map + `bindStyle` 已包含 Typography2025 全部样式。`bindStyle` 是 **async** 函数，必须 `await`。

```js
await bindStyle(titleNode, 'Semibold/20');
await bindStyle(bodyNode, 'Regular/14');
```

> **注意**: 32px Hero 字号没有对应的库样式，需手动设 fontSize + fontWeight。
> **⚠️ overrideText 自动保存/恢复 textStyleId。** 但如果直接设 `t.fontName` + `t.characters`（不通过 overrideText），必须手动保存和恢复 `t.textStyleId`，否则字体样式绑定会丢失。

### ⛔ 100px 默认尺寸问题（CRITICAL — 必须防范）

Figma 中空的 auto-layout frame 设 `layoutSizingVertical/Horizontal = 'HUG'` 时，因为没有 children 撑开，**默认回退到 100px**。这是 Figma 引擎行为，会导致页面出现不期望的 100px 空白。

**⛔ 三层防御（Standard Helpers 已内置前两层）：**

**第一层：用 `makeFrame` + `setHug` 代替手动创建**
```js
// ❌ WRONG — 手动创建，容易忘记顺序
const section = figma.createFrame();
section.layoutMode = 'VERTICAL';
parent.appendChild(section);
section.layoutSizingVertical = 'HUG'; // 此时无 children → 100px!

// ✅ CORRECT — 用 makeFrame（不设 HUG），加完 children 后 setHug
const section = makeFrame('Section', parent, { spacing: 16, padding: 20 });
section.appendChild(child1);
section.appendChild(child2);
setHug(section); // children 都加完了，现在安全设 HUG
```

**第二层：禁止空 spacer frame**
```js
// ❌ WRONG — 空 frame 做间距
const spacer = figma.createFrame(); // → 100px!

// ✅ CORRECT — 用父级 itemSpacing 控制间距
parent.itemSpacing = 24;
```

**第三层：`fix100px(root)` 兜底清除（已内置于 Standard Helpers）**
```js
// ⛔ 每个 use_figma 脚本 return 前必须调用
const fixedCount = fix100px(root);
return { ..., fixedCount };
```

> **JSON Mode (Mode B) 同样适用**：在 JSON 中不要生成空的 frame 节点做间距。用 `itemSpacing` 和 `paddingTop/Bottom` 控制。

### 其他 Gotchas

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| `layoutSizingHorizontal = 'FILL'` 报错 | 在 `appendChild` 之前设置 | 先 `appendChild`，再设 sizing |
| 组件文案不更新（仍显示 "Primary"/"Page_Title"） | 没先设 `fontName` | 必须先 `t.fontName = {...}` 再 `t.characters = 'xxx'` |
| Font "Inter SemiBold" 加载失败 | Figma 用 "SemiBold" 但 API 要 "Semi Bold" | `loadFont` 已自动处理，返回实际加载的 style name |
| 图标颜色绑定无效 | Icon 实例 `fills = []`，颜色在子节点 | 用 `findAll` 找内部有 solid fill 的子节点 |
| Nav Bar 右侧 Action 按钮显示 | 默认显示 | `nav.setProperties({ 'Show Right#758:15': false })` |
| 📝 `overrideText` 静默失败，文案没变 | 组件 text node 的 `characters` 不含 searchText（如 Tab 的 characters 是 "ALL" 而非 "Tab"） | 已修复：`overrideText` 现在先匹配 characters，再匹配 node name |
| 📝 Token Logo key 报 "not found" | Common Components 表的 key 可能过期（light/dark 混淆） | 先用 `importComponentByKeyAsync` 测试，失败时从 `token-logos.md` Dark 列查正确 key |
| 📝 overrideText 后截图仍显示旧文案 | `loadFont` 加载了 "Semi Bold"，但 `fontName` 仍设为旧值 | 已修复：`loadFont` 返回实际加载的 style，`overrideText` 用返回值设 `fontName` |

### 增量构建模板

```js
// === Step 1: Root + Nav + Content + Bottom ===
// 切换到目标页面
const pages = figma.root.children;
const page = pages.find(p => p.id === 'TARGET_PAGE_ID');
await figma.setCurrentPageAsync(page);

// 创建 Root
const root = figma.createFrame();
root.name = 'Page Name';
root.resize(393, 852);
root.layoutMode = 'VERTICAL';
root.itemSpacing = 0;
root.primaryAxisSizingMode = 'FIXED';
root.counterAxisSizingMode = 'FIXED';
root.clipsContent = true;
root.fills = [{ type: 'SOLID', color: {r:0,g:0,b:0} }];

// 导入 Nav Bar
const navComp = await figma.importComponentByKeyAsync('9c90c04891f54f6dceb072f3ef79378ee3eb12ea');
const nav = navComp.createInstance();
root.appendChild(nav);
nav.layoutSizingHorizontal = 'FILL';

// 创建 Content area
const content = figma.createFrame();
content.name = 'Content';
content.layoutMode = 'VERTICAL';
content.itemSpacing = 16;
content.paddingLeft = 20; content.paddingRight = 20;
content.paddingTop = 24; content.paddingBottom = 24;
content.fills = [];
root.appendChild(content);
content.layoutSizingHorizontal = 'FILL';
content.layoutSizingVertical = 'FILL';

return { rootId: root.id, navId: nav.id, contentId: content.id };
```

---

## Mandatory Rules

These rules are **non-negotiable** and apply to every design you generate (both Mode A and Mode B).

### Layout & Structure

1. **Auto Layout mandatory** — Every `frame` must have `layoutMode` (`VERTICAL` or `HORIZONTAL`). Never use absolute positioning.
2. **Buttons Full Fill** — All button instances must have `"layoutSizingHorizontal": "FILL"` to fill parent width. Exception: small buttons inside nav bar.
3. **StatusBar mandatory on every page** — Every page must have a StatusBar at the top. There are two cases:
   - **Page has Nav Bar** → Nav Bar already includes a built-in StatusBar. Do NOT add a separate StatusBar instance (it would create a duplicate "9:41" display). The Nav Bar must be the first child of Root.
   - **Page has NO Nav Bar** (e.g., Earn home, full-screen overlay, onboarding, or any page using a custom header instead of Nav Bar) → Add a standalone StatusBar instance as the **first child** of Root:
   ```jsonc
   // First child of Root when there is no Nav Bar:
   { "type": "instance", "componentKey": "53ba299ab451d17401de13745980be73647e7493", "name": "StatusBar", "layoutSizingHorizontal": "FILL" }
   ```
   **⛔ No page may render without a StatusBar. This is a BLOCKING rule — verify before outputting JSON.**
4. **No Card/Title_Inside component** — Section headers use custom `HORIZONTAL` frame (title text + chevron icon), not Card component variants.

### Localization

5. **Always output in English** — Regardless of the reference image's language (Chinese, Japanese, Korean, etc.), all text content in the generated JSON must be translated to English. This includes titles, descriptions, button labels, tags, tooltips, disclaimers, and any other user-facing text.

### Colors & Fills

6. **Figma fills don't support `a` (alpha)** — Use `"color": {"r": ..., "g": ..., "b": ...}` with a separate `"opacity"` field if needed. Never include `"a"` in color objects.
7. **Color variable dual write** — Always include both variable key AND RGB fallback: `"colorVariable"` + `"color"`, or `"backgroundVariable"` + `"background"`.
8. **Card/frame backgrounds must use standard tokens only — NEVER use arbitrary RGB.** All container backgrounds must come from the design token system。仅允许 `Bg_Page`、`Bg_Card_App`、`Bg_Float`、`Ele_line`、`Brand/700`（CTA only）五种背景 token（完整 Variable Key + RGB 见底部「核心颜色 Tokens」表的「背景」行）。

    **❌ 禁止**: 自定义 RGB 作为背景色（如 `{"r":0.02,"g":0.1,"b":0.06}` 自定义绿）。想表达涨跌含义时，只在**文字颜色**上使用 Green/Red token，容器背景保持 `Bg_Card_App`。

### Icons

9. **All icons from component library** — Use `componentKey` from `components/icons.md` (518 `icon_*` + 93 `BL_` icons). Never use emoji or draw custom icons. **Every icon key must be looked up from `icons.md` — see Rule 14.**
10. **nodeId is deprecated** — Icons use `componentKey` only (migrated 2026-03-24). Never use `nodeId`.
11. **Icon max size 32px (including BL_)** — Standard: 24×24, small: 20/16/12, max: 32×32. **All icons including BL_ Business Line icons** must not exceed 32px. Never set icons to 48/56/64px.
12. **BL_ icons need card background** — Business Line (BL_) colorful icons must sit inside a `#141414` card container (`cornerRadius: 12`), never on pure black `#000000`.

### Icon Selection by UI Pattern

Icons with similar names render very differently. **Always match by UI pattern, not by guessing the name.**

| UI Pattern | Correct Icon | componentKey | Wrong Choice (DO NOT use) |
|-----------|-------------|-------------|--------------------------|
| Dropdown triangle ▼ (selector, filter) | `icon_arrow down` | `9c49da4607ea1b289b1c8d4de2fdf21d704b76ea` | ~~icon_arrow chevron down~~ (renders as open V) |
| List row chevron > (navigate to detail) | `icon_arrow chevron right` | `bf46077ee4bce2ce82e70b8489e52381994cfb99` | ~~icon_arrow right~~ (renders as filled ▶) |
| Text link arrow → (View All, Log In, etc.) | `icon_arrow chevron right` | `bf46077ee4bce2ce82e70b8489e52381994cfb99` | ~~icon_arrow right~~ / ~~icon_direction right~~ |
| Direction arrow → (explicit navigate) | `icon_direction right` | `5927103d8bb62fbc0cd0020d8b1cbb106f57bcfe` | — |
| Expand/collapse ∨ (accordion, panel) | `icon_arrow chevron down` | `7257262c737c3196d0092d97e96796e8d3fa2dd7` | ~~icon_arrow down~~ (renders as filled ▼) |
| Back arrow ← (nav bar built-in) | Nav Bar component handles this | — | Don't manually add |
| Close × | `icon_close` | `74cecbad1b458074fd61d1cf662fae6ff43cb59a` | — |
| Info tooltip ⓘ | `icon_info circle` | `0abdbba38b55d7dbf5a7603b344ecbdbb3ce1eb6` | — |
| Help/question ⓘ | `icon_help` | `4f17fd07612dddec447926d3b8ecdbb8e339501f` | — |
| Swap/transfer ⇄ | `icon_exchange` | `3788eff77bcff099ead243139cebf24926611ba2` | — |
| Search 🔍 | Use Search component | — | Don't use icon alone |

> **Rule of thumb**: `icon_arrow *` = filled/solid arrows, `icon_arrow chevron *` = outlined/line arrows. Dropdown selectors use filled (▼), list navigation uses outlined (>).

### Result / Status Icons

13. **Status icons — use Result State component, not Result component** — When displaying a success/error/warning/pending status icon on a page (e.g. checkmark on a success page), use the **Result State** component set below. Do NOT use the `Result` component (it includes Title + Description that cannot be hidden). Do NOT use plain `icon_check circle` either — always use Result State for consistent styling.

    | State | componentKey | Use for |
    |-------|-------------|---------|
    | Success | `181a1db130b61d0b121886375ce5b5bd5477af64` | 成功状态（绿色动效勾） |
    | Error | `1309910fff41874fe7f44fe50b8613dce1d8bbd4` | 失败/错误状态 |
    | Warning | `e920c5b77ac7715e86357bda8bf04d57ab9da55c` | 警告状态 |
    | Pending | `81ba9049eb99806977030e3ecd07ed992a215a25` | 等待/审核中状态 |
    | Processing | `c25869b610e8ff9cc240d82b787c4fc6a68982fd` | 处理中（带计数器） |

    ```jsonc
    // Example: Success icon on a result page
    { "type": "instance", "componentKey": "181a1db130b61d0b121886375ce5b5bd5477af64",
      "name": "Result State Success" }
    ```

    > **Only use `Result` component inside Dialog (居中弹窗)** where title + description text are also needed.

### Component Key Integrity (CRITICAL)

14. **NEVER write a componentKey from memory — ALWAYS look it up from source files.** This is the #1 cause of build failures (S3/S5/S7/S11/S15/S17/S19/S22/S28 — 9 out of 28 test issues). Every single `componentKey` in the JSON must be **copy-pasted from the component files**, never typed from memory or guessed.

    **此规则通过 Workflow 步骤 5「🔑 查表取 Key」强制执行。** 在写 JSON 之前，必须先产出 Key 查取结果表（Component → Source File → componentKey → Override Keys）。没有这张表 = 不能开始写 JSON。

    **两种合法的 Key 来源：**
    1. **Common Components 表**（本文件中已验证的高频组件）— 可直接引用
    2. **组件源文件**（`buttons.md`、`icons.md`、`tags.md`、`controls.md` 等）— 必须 Read 文件后复制

    **Common violations (all result in broken red squares in Figma):**
    - "I remember the icon key" → Wrong. Read `icons.md`.
    - "The checkbox key should be..." → Wrong. Read `controls.md`.
    - "This slider key looks right" → Wrong. Read `slider.md`.

    **If a key is not in the Common Components table above, it MUST be looked up from the component file. No exceptions.**

### Asset Header (资产头部)

15. **Asset/Balance headers go directly on page background — NO card wrapper. EVEN IF the reference image uses a card.** When a page has a single key value at the top (balance, equity, total assets) with supporting stats (yield, profit, change %), the entire header section must be placed directly on `Bg_Page` (#000) as flat layout — **never wrapped in a `Bg_Card` card or any container with a visible background**. This is a mandatory Bybit App pattern. The reference image's card styling must be ignored.

    **⛔ This is a BLOCKING rule — reference images showing card-wrapped balance headers do NOT override this rule.**

    **判断规则:**
    | 内容类型 | 处理方式 |
    |---------|---------|
    | 页面头部的主余额 (Total Equity, Balance) | 直接放页面背景上，32px/600 Hero 字号，**无卡片包裹** |
    | 余额下方的统计数据 (Yesterday's Yield, Total Profit, Available, In Use) | 同属头部，直接放页面背景上，**无卡片包裹**，用 Ele_line 背景的小色块区分即可 |
    | 头部区域的 CTA 按钮 (Invest Now 等) | 同属头部，直接放页面背景上 |
    | 中间的产品卡、功能卡 | 包裹 Card (Bg_Card #141414) |

    **结构模板（Asset Header on Bg_Page）：**
    ```
    Content (VERTICAL, on Bg_Page #000)
      +-- Label row ("Total Equity (USD)" + eye icon) — T2 color
      +-- Hero value ("12,458.20") — 32px/600 T1 white
      +-- Stat pills row (HORIZONTAL, gap 12)
      |     +-- Pill 1 (Ele_line bg, cornerRadius 12, padding 12-16) — label T2 + value Green
      |     +-- Pill 2 (same pattern)
      +-- CTA Button (Button_Primary XL, FILL)
    ```

### Components vs Custom

16. **Prefer library components — never hand-draw what exists as a component.** Before building any UI element as a custom frame, check the component library. The following UI patterns **must** use library components:

    | UI Element | Must Use | Component File | DO NOT hand-draw |
    |-----------|----------|---------------|-----------------|
    | Text input field | `Input_Default` or `Input_Compact` | `inputs.md` | Custom frame with placeholder text |
    | Search bar | `SearchBar` | `search.md` | Custom frame with search icon |
    | Toggle switch | `Switch` | `controls.md` | Custom frame with circle |
    | Checkbox / Radio | `Checkbox` / `Radio` | `controls.md` | Custom frame with checkmark |
    | Dropdown selector | `Dropdown_Selector` | `dropdowns.md` | Custom frame with dropdown arrow |
    | Horizontal divider | `Divider` | `dividers.md` | Custom `line` node (except inside swap/transfer patterns) |
    | Price change badge | AutoLayout 自绘涨跌标签 | `tags.md` | Custom colored text |
    | Bottom button + Home Bar | `Button_Group` | `buttons.md` | Manual Button + iOS Home Bar |
    | Alert / Warning banner | `Alert` | `alert.md` | Custom colored frame (unless the design is highly customized) |
    | Tab bar / Tab switching | `Tab` (L1/L2/L3) | `tabs.md` | Custom frame with text + underline |
    | Buy/Sell toggle / binary choice | `Tab_Trade` (Tab_Average_text) | `tabs.md` | Custom segmented control |
    | Action button (Transfer, Withdraw, Deposit, etc.) | `Button_Secondary` (L/M) | `buttons.md` | Custom frame with icon + text |
    | CTA / Submit button | `Button_Primary` (XL/L) | `buttons.md` | Custom orange frame |
    | Buy / Sell action | `Button_Trading` | `buttons.md` | Custom green/red frame |
    | Image placeholder | `Picture` | Common Components | Custom gray frame with text |

    > **原则**: 只要是按钮交互（可点击、有操作含义），必须用 Button 组件。即使参考图的按钮样式与组件不完全一致（如带 icon），也应优先使用组件，牺牲 icon 以保证组件关联和设计一致性。

17. **Tags & Labels must be component instances** — All Tags/Badges use `Tag_Sales`/`Tag_Trading` components. Always override placeholder text: `"overrides": {"Tag": "ACTUAL TEXT", "Label": "ACTUAL TEXT"}`. **涨跌幅标签例外** — 用 AutoLayout 自绘（见 tags.md），不用组件。**涨跌幅标签全圆角规则：cornerRadius 永远 = height / 2（28高→14，24高→12）。绝不用 4 或 8。**

    **⚠️ Override 值必须是实际内容，绝不能是占位符!** `"Tag"`, `"Label"`, `"Text"`, `"Title"` 等都是组件默认的占位符文字。Override 的值必须替换为页面的真实文案。
    ```jsonc
    // ❌ WRONG — 占位符原封不动
    "overrides": {"Tag": "Label", "Label": "Label"}
    
    // ✅ CORRECT — 实际文案
    "overrides": {"Tag": "NEW", "Label": "NEW"}
    "overrides": {"Tag": "HOT", "Label": "HOT"}
    "overrides": {"Tag": "PROMO", "Label": "PROMO"}
    ```

    **Tag Shape Selection:**
    | 场景 | Shape | 放置方式 |
    |------|-------|---------|
    | 行内标签（跟在文字后面） | **Default**（圆角矩形） | 在 HORIZONTAL row 内，和文字同行 |
    | 卡片角标（右上角状态标签，如 "In Use"、"Recently Used"） | **Leaf**（叶形） | 卡片改为 VERTICAL，第一行 Tag Row（右对齐），第二行 Card Content |

    **卡片角标 Leaf Tag 模式：**
    ```jsonc
    // Card 改为 VERTICAL，Tag 贴右上角
    { "type": "frame", "name": "Card", "layoutMode": "VERTICAL", "itemSpacing": 0,
      "cornerRadius": 12, "clipsContent": true,
      "background": ..., "children": [
        // 1. Tag Row — 右对齐，无 padding
        { "type": "frame", "name": "Tag Row", "layoutMode": "HORIZONTAL",
          "layoutSizingHorizontal": "FILL", "primaryAxisAlignItems": "MAX",
          "children": [
            { "type": "instance", "componentKey": "c45b8a35...(Leaf S)",
              "overrides": {"Tag": "In Use"} }
          ] },
        // 2. Card Content — 实际内容，有自己的 padding
        { "type": "frame", "name": "Card Content", "layoutMode": "HORIZONTAL",
          "paddingLeft": 16, "paddingRight": 16, "paddingTop": 4, "paddingBottom": 16,
          "children": [ /* Avatar, Info, etc. */ ] }
      ] }
    ```
18. **Token logos from library first** — 1289 coin logos in `components/token-logos.md`. Only use `crypto-logo-to-file` skill when the token is missing from the library.
19. **Mix components and custom frames** — Use components where they exist, custom frames where needed. Don't force everything into components or everything into custom frames.
20. **Component labels use overrides, not extra text nodes** — Components with built-in text labels (Checkbox, Radio, Switch, etc.) must set their label via `overrides`. **Never** place a separate `text` node next to the component.
    ```jsonc
    // ✅ CORRECT — override the built-in Label
    { "type": "instance", "componentKey": "21e3d584...", "overrides": {"Label": "Funding"} }
    
    // ❌ WRONG — separate text node creates "Label Funding"
    { "type": "instance", "componentKey": "21e3d584..." },
    { "type": "text", "content": "Funding" }
    ```

### Tab 层级与组合规则

21. **Tab 层级严格递减，禁止跳级搭配。** Tab 分 L1（页面级）、L2（主题级）、L3（分类级）三个层级。使用时必须遵守以下规则：

    **层级语义（选型依据）：**
    | Level | 语义 | 说明 |
    |-------|------|------|
    | L1 | 页面级 | 整个页面所有内容归属于此 Tab |
    | L2 | 主题级 | 同一主题下内容维度切换（列表、仓位） |
    | L3 | 分类级 | 信息分类维度切换。**⛔ 不能单独出现** |

    **⛔ 强制规则：**
    - L3 **禁止单独使用** — 必须有 L1 或 L2 作为上层
    - 多层 Tab 必须 **尺寸递减**（上层 > 下层）
    - **禁止跳级**：L1 不能直接跳到 L3，Size L 不能直接跳到 Size S

    **合法的二层搭配（仅这 3 种）：**
    | 上层 | 下层 | 场景 |
    |------|------|------|
    | L1-L (16px) | L2-M (14px) | 行情页等大信息层级 |
    | L2-L (16px) | L3-M (14px) | 订单记录、仓位列表 |
    | L2-M (14px) | L3-S (12px) | 卡片等较小空间 |

    **合法的三层搭配（仅 1 种）：** L1-L → L2-M → L3-S

    **❌ 禁止：** L1→L3（跳级）、L→S（尺寸跳级）、下层≥上层（违反递减）

    > 完整规范见 `tabs.md`。

---

## JSON Spec Format

```jsonc
{
  "name": "Page Name",
  "width": 393,                    // iPhone 14 width (default)
  "height": 852,                   // iPhone 14 height (default)
  "background": {"r": 0, "g": 0, "b": 0},  // dark theme default
  "layoutMode": "VERTICAL",
  "itemSpacing": 0,
  "primaryAxisSizingMode": "FIXED",
  "counterAxisSizingMode": "FIXED",
  "clipsContent": true,
  "children": [...]
}
```

### Node Types

| type | 用途 | 关键字段 |
|------|------|----------|
| `instance` | 组件库实例 | `componentKey`, `overrides`, `width`, `layoutSizingHorizontal`, `colorVariable` |
| `text` | 自定义文本 | `content`, `fontSize`, `fontWeight`, `color`, `colorVariable`, `layoutSizingHorizontal` |
| `frame` | 容器/布局 | `layoutMode`, `itemSpacing`, `padding*`, `children`, `background`, `backgroundVariable`, `cornerRadius`, `layoutSizingHorizontal`, `layoutSizingVertical` |
| `rect` | 矩形 | `width`, `height`, `color`, `colorVariable`, `cornerRadius` |
| `line` | 线条 | `width`, `height`, `color` |

> **Limitation**: The plugin does NOT support `stroke` (borders) on any node type. All colors are applied as `fills`. Use Divider component or `line` node for visual separators.

### Instance Node

```jsonc
{
  "type": "instance",
  "componentKey": "89f2908aa19229b066a1caf11423063a51578f41",
  "name": "Button Name",                    // layer name in Figma
  "width": 361,                              // optional, resize width
  "height": 48,                              // optional, resize height
  "layoutSizingHorizontal": "FILL",          // fill parent width (preferred)
  "layoutSizingVertical": "HUG",             // hug content height
  "overrides": {
    "Override Key": "替换文案"                // see Override Rules below
  },
  "iconOverrides": {                           // swap nested icon instances (optional)
    "icon_name": {
      "componentKey": "new_icon_key",
      "color": {"r": 1, "g": 1, "b": 1},     // optional tint
      "colorVariable": "variable_key"          // optional variable
    }
  }
}
```

**Icon Override** — Replace nested icon instances inside a component (e.g. Nav Bar right-side icons):

```jsonc
{
  "type": "instance",
  "componentKey": "38d96d007ec5b7d3623ae2c4f46e62b30842e859",  // Nav Bar Arrow+Icons
  "name": "Navigation Bar",
  "overrides": { "Page_Title": "My Title" },
  "iconOverrides": {
    "icon_refresh": {
      "componentKey": "4f17fd07612dddec447926d3b8ecdbb8e339501f",  // icon_help
      "color": {"r": 1, "g": 1, "b": 1},
      "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51"
    },
    "icon_more": {
      "componentKey": "d4b3e6c9b74556b95e8da53352f42db679a37f23",  // icon_list
      "color": {"r": 1, "g": 1, "b": 1},
      "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51"
    }
  }
}
```

Matching rules (same as text overrides): 1) exact name, 2) name includes, 3) main component name includes. Target key is the **current** icon name inside the component.

**Icon instance example** — Icons now use `componentKey` (same as other components):

```jsonc
{
  "type": "instance",
  "componentKey": "f5e03519b1c2c9e3c1d092c9492100d8238f1952",  // icon_lightning
  "name": "icon_lightning",
  "width": 24, "height": 24,
  "colorVariable": "0d73577f4ec792b83e33db70b6d3929d17925379",  // Brand/700-normal variable
  "color": {"r": 1, "g": 0.612, "b": 0.18}  // fallback RGB
}
```

**Icon color** (`color` + `colorVariable` fields): `colorVariable` binds to Figma library color variable; `color` is the RGB fallback. Plugin tries variable first, falls back to raw RGB. **All icon colors must reference color tokens from the component library (Section 1 of design-rules.md). Always include both `colorVariable` and `color`.**

**Icon color rules (IMPORTANT) — must use library color tokens (见 Quick Reference 文字色表):**

| Role | Token | colorVariable |
|------|-------|---------------|
| Primary icon | T1_Title (白) | `0660...e51` |
| Secondary icon | T3 (暗灰) | `4d26...9e` |
| Up/Buy trend only | Green/700 | `a7c8...c1` |
| Down/Sell trend only | Red/700 | `385d...35` |
| Brand logo only | Brand/700 | `0d73...79` |

> 完整 colorVariable + RGB fallback 见本文件底部「核心颜色 Tokens」表。此处仅列角色映射。

**Icon usage rules**:
1. Always use iconfont icons from `components/icons.md` via `componentKey`. Do NOT use emoji text as icon replacements.
2. Always set `color` — never leave icons uncolored.
3. Most icons use `T1` (white). Secondary hints use `T3`. Green/Red ONLY for 涨跌. Brand orange ONLY for logo.
4. Do NOT use arbitrary RGB — always map to a library color token.
5. Full specification: `components/design-rules.md` Section 1.5.


### Auto Layout Properties (for frame and root)

```jsonc
{
  "layoutMode": "VERTICAL" | "HORIZONTAL",
  "itemSpacing": 16,
  "primaryAxisAlignItems": "MIN" | "CENTER" | "MAX" | "SPACE_BETWEEN",
  "counterAxisAlignItems": "MIN" | "CENTER" | "MAX",
  "paddingLeft": 16,
  "paddingRight": 16,
  "paddingTop": 16,
  "paddingBottom": 16
}
```

> **Root-only**: `primaryAxisSizingMode` / `counterAxisSizingMode` are only needed on the **Root frame** (which has no parent auto layout). Child frames use `layoutSizingHorizontal` / `layoutSizingVertical` instead.

### Layout Sizing (CRITICAL — for all child nodes in auto layout)

Use `layoutSizingHorizontal` and `layoutSizingVertical` to control how child nodes size themselves within their parent's auto layout. **Always prefer these over the legacy `layoutAlign`/`layoutGrow` API.**

```jsonc
{
  "layoutSizingHorizontal": "FILL" | "HUG" | "FIXED",  // how this node sizes horizontally
  "layoutSizingVertical": "FILL" | "HUG" | "FIXED"     // how this node sizes vertically
}
```

| Value | Meaning | Figma equivalent |
|-------|---------|-----------------|
| `"FILL"` | Fill parent's available space on this axis | "Fill container" |
| `"HUG"` | Shrink to fit own content on this axis | "Hug contents" |
| `"FIXED"` | Use explicit `width`/`height` value | "Fixed" |

**Common patterns:**

| Scenario | layoutSizingHorizontal | layoutSizingVertical |
|----------|----------------------|---------------------|
| Full-width row/section | `"FILL"` | `"HUG"` |
| Content area (fills remaining height) | `"FILL"` | `"FILL"` |
| Text that wraps to fill width | `"FILL"` | _(not needed)_ |
| Fixed-size icon | _(not needed, use `width`)_ | _(not needed, use `height`)_ |
| Button filling parent width | `"FILL"` | _(not needed)_ |

**Text wrapping**: Setting `layoutSizingHorizontal: "FILL"` on a text node automatically enables text wrapping (the plugin sets `textAutoResize = 'HEIGHT'`). No need for explicit `width` on text nodes — just use `"FILL"`.

> **Legacy API note**: `layoutAlign: "STRETCH"` and `layoutGrow: 1` are still supported for backward compatibility but should NOT be used in new JSON specs. Always use `layoutSizingHorizontal`/`layoutSizingVertical` instead — they are more explicit and reliable.

### Text Node

Always include **both** `textStyle` and manual fallback (`fontSize`/`fontWeight`/`lineHeight`). Plugin tries `textStyle` first (binds Typography2025 if available locally), falls back to manual values.

```jsonc
{
  "type": "text",
  "content": "Hello World",
  "textStyle": "Regular/14",        // Typography2025 — applied if style exists locally
  "fontSize": 14, "fontWeight": 400, "lineHeight": 22,  // fallback — always works
  "color": {"r": 1, "g": 1, "b": 1}
}
```

**Typography2025 variables (App-relevant):**

| textStyle | fontSize | fontWeight | lineHeight | Use for |
|-----------|----------|------------|------------|---------|
| `Semibold/24` | 24 | 600 | 32 | Page titles |
| `Semibold/20` | 20 | 600 | 28 | Section titles (bold) |
| `Regular/20` | 20 | 400 | 28 | Section titles (regular) |
| `Semibold/18` | 18 | 600 | 26 | Card titles |
| `Regular/18` | 18 | 400 | 26 | Large body |
| `Semibold/16` | 16 | 600 | 24 | Key metrics, last price |
| `Regular/16` | 16 | 400 | 24 | Default body font |
| `Semibold/14` | 14 | 600 | 22 | Active tab, button text |
| `Regular/14` | 14 | 400 | 22 | Body text, descriptions |
| `Semibold/12` | 12 | 600 | 18 | Small bold label, tag |
| `Regular/12` | 12 | 400 | 18 | Helper text, footer |
| `Semibold/10` | 10 | 600 | 14 | Micro bold |
| `Regular/10` | 10 | 400 | 14 | Micro text |
| `Semibold/8` | 8 | 600 | 10 | Badge text only |
| `Headline/H0` | — | — | — | Hero display (largest) |
| `Headline/H1` | — | — | — | Page hero title |
| `Headline/H2` | — | — | — | Major section title |
| `Headline/H3` | — | — | — | Sub-section title |
| `Headline/H4` | — | — | — | Card/block title |
| _(none)_ | 11 | 400 | 16 | Orderbook, compact rows |
| _(none)_ | 13 | 400/600 | — | Nav links, small tabs |

Optional fields: `textAlignHorizontal` (LEFT/CENTER/RIGHT), `width`, `fontFamily`, `letterSpacing`

**Text wrapping**: For text that should fill the parent width and wrap automatically, use `"layoutSizingHorizontal": "FILL"` instead of setting an explicit `width`. The plugin automatically enables `textAutoResize = 'HEIGHT'` for FILL text nodes. Only use explicit `width` when you need a specific fixed width (rare).

---

## Variant Selection Defaults

Always pick Dark Mode variant unless user specifies light theme.

| Prop | Default | When to change |
|------|---------|----------------|
| Dark Mode | **On / Yes** | Only if user says "light theme" |
| Status/State | **Normal** | Disable/Loading/Error only if showing that state |
| Size | **XL** for primary CTA, **L** for secondary, **M** for compact, **S** for inline | Match context |
| Trend | **Up** (green) | Down for sell/loss, Unrelated for neutral |

## Common Components (Dark Mode, Normal)

Use these directly without reading category files:

| Component | Key | Use for |
|-----------|-----|---------|
| StatusBar | `53ba299ab451d17401de13745980be73647e7493` | 状态栏(无 Nav Bar 时必加) |
| Navigation Bar (Arrow+Text) | `9c90c04891f54f6dceb072f3ef79378ee3eb12ea` | 顶部导航栏(含内置 StatusBar) |
| Tabbar (Home) | `6feb5e098101edce2fc2b01967d3dc308babe5b3` | 底部导航 |
| Button_Primary XL | `6f623a2d962d0e04658039c1bbaa20a66379b635` | 主按钮 |
| Button_Primary L | `b8da98951051e5bf3d37a24310be1c467786e6f6` | 次级主按钮 |
| Button_Secondary XL | `ad6c561802c9fbc6846d153cf48ec51ad4f818fd` | 次级按钮(描边) |
| Button_Secondary L | `808963e35a0b45fb18bbcdd3cb34c493e5011dd1` | 次级按钮 L |
| Button_Secondary S | `ced899c42d49a3447ec64cb36bda7b1d5fd7c865` | 次级按钮 S |
| Button_Trading Buy XL | `1786d399ac5ccdf07d220122157c5b8da0e39f11` | 买入按钮 |
| Button_Trading Sell XL | `573da35491e90705195c2988ae6f6fdf0b0e4db2` | 卖出按钮 |
| Input_Default 48px Dark | `8f621392ad9ab4bf1e8d2db641c8211bac9dc9f7` | 标准输入框 |
| Input_Compact 40px Dark | `a919553b56721a68d710834ef18643a9cdb7ed68` | 紧凑输入框 |
| SearchBar (No Cancel) | `c67d06d18e0cd0a953bef2e8af76c6165a2b4f04` | 搜索框 |
| SearchBar (With Cancel) | `cd3e22afcfb31a956cb8562f430bcb32d7da89c1` | 搜索框(带取消) |
| Divider 0.5 Line Dark | `27c32b9e3c92dfd8640623ff6a23443cf44c518c` | 分割线 |
| icon_close | `74cecbad1b458074fd61d1cf662fae6ff43cb59a` | 关闭按钮 |
| Panel Handle | `8ba02c67aec6cf3dee2f96822216e2fccfa8dd45` | Panel 顶部拖拽条 |
| .iOS_Home Bar | `f98cab636e44f9000be92bbb96cfbf3addb81c8e` | 底部 Home 指示条 |
| Button_Group (No.=1) | `ee497740062678d11225cabca6460a60dce92461` | 单按钮组(含 Home Bar) |
| Button_Group (No.=2) | `fb247c6b7c952ed7823f974d138d24a5ab7c2572` | 双按钮组垂直(含 Home Bar) |
| Button_Group (2H) | `fe01415f617a6713698adda8b28dbd0179fdf66c` | 双按钮组水平(含 Home Bar) |
| **Icons (高频)** | | |
| icon_arrow left | `49b7f66d9e7c67518941a11bb696312a0cf482fb` | 返回箭头 |
| icon_arrow chevron right | `bf46077ee4bce2ce82e70b8489e52381994cfb99` | 列表行右箭头 > |
| icon_arrow chevron down | `7257262c737c3196d0092d97e96796e8d3fa2dd7` | 展开/折叠 ∨ |
| icon_arrow down | `9c49da4607ea1b289b1c8d4de2fdf21d704b76ea` | 下拉三角 ▼ |
| icon_more horizontal | `5cc9c49680a525b47e3b4ccdf73a18b344b7df67` | 更多(横) ··· |
| icon_candle chart | `88c1d386eebb6771a28d676494712369ab0c72d1` | K线图 |
| icon_copy | `c8a47b4ec1a134b9e960871a020926e4d975d60a` | 复制 |
| icon_setting | `3edc71b6b286cb0797345b91a579db91dccea610` | 设置齿轮 |
| icon_info circle | `0abdbba38b55d7dbf5a7603b344ecbdbb3ce1eb6` | 信息提示 ⓘ |
| icon_eye on | `af9072975549364d99eb9059b93b545f8e39691a` | 显示/眼睛 |
| icon_search | `ac33a97b8c0ad940c2034de052635be09ea86c81` | 搜索 |
| icon_bell | `45ed8ffd068f6d20d3dc5931d9060e8035aae5c0` | 通知铃铛 |
| icon_user | `0f6193b005acfe120443da78fc1e4751c4b1e2b8` | 用户/头像 |
| icon_fire | `d2b11a135060dd71ecbb370e1adc827b19d66203` | 热门/trending |
| icon_wallet | `683c36d5a9be67dafe14bc4ccac4bd8e300d3c1b` | 钱包 |
| icon_lightning | `f5e03519b1c2c9e3c1d092c9492100d8238f1952` | 闪电/快速 |
| icon_exchange | `3788eff77bcff099ead243139cebf24926611ba2` | 兑换/划转 |
| **Tabs (高频)** | | |
| Tab L2 Active M | `d112f31d85d5a2140b17f3f57bde06ccd01c8be6` | Tab 激活态(14px) |
| Tab L2 Deactive M | `73d6933f0bfe4587bd5db125a0c933f8223236d9` | Tab 非激活态(14px) |
| Tab L1 Active L | `af31bba292dc163ef4408a005f606853981f344a` | Tab 页面级激活(16px) |
| Tab L1 Deactive L | `84216841063d054676c124b4b634ff3ef82ab24c` | Tab 页面级非激活(16px) |
| **Token Logos (Top 5, Dark)** | | |
| BTC Logo | `5998256674af45aeef4442a4499fbed8dfabf811` | 比特币 |
| ETH Logo | `a543b31d068f27d8891e8196568e61d935bf3eeb` | 以太坊 |
| SOL Logo | `95950a05ea15f0fb9b7244c01d6daf7aab2a00e2` | Solana |
| XRP Logo | `9f44c2734adb066c2ce76cb830ccc072e51cba21` | XRP |
| DOGE Logo | `a920fdd7b5ef5e9732e7eb9b7500255348d9675d` | 狗狗币 |
| USDT Logo | `6144f7785645d910da6fa42b3bf956bba001ba59` | Tether (dark — 📝 2026-04-13 修正) |
| USDC Logo | `25ce2e4965ba09370de503a2a40368b157fabca2` | USD Coin (dark — 📝 2026-04-13 修正) |
| **Controls (高频)** | | |
| Switch L On | `1acc910f0242ffd50cfe5eae3f07ef04c8df973d` | 开关(开) |
| Switch L Off | `c151681df79878443ab92f0134f19cf3f55e5ecb` | 开关(关) |
| Checkbox Unchecked | `8a1cb693d9617f67fdc1f2e6025a5656bf1d3bf1` | 复选框(未选) |
| Checkbox Checked | `21e3d58494dbc1b4e3ff19e1f7444cfb62208ee5` | 复选框(已选) |
| Radio Unchecked | `a06e259122d7d63a6317202996fd4c449641c1fe` | 单选(未选) |
| Radio Checked | `6cd457baf7f5ba6ce186fb37ebbc90af74d6dddb` | 单选(已选) |
| **Slider (高频)** | | |
| Slider Drag 0% | `826e1d437d282d8e1d3b56c6a738d5af406924e1` | 滑块起始 |
| Slider Drag Custom | `26deade7fbab7079b988a8ef2ed62c127f84acdc` | 滑块中间 |
| Slider Drag 100% | `5f00691a36fdaac38ef0d593cbb892cf736e4bc9` | 滑块满 |
| **Placeholder (高频)** | | |
| Picture | `4b04881f37fdc13783e6457e62a8dde8e6756e50` | 图片占位组件 |

---

## Component Key Lookup

组件库分 4 类共 28 个文件，按需读取。完整索引见 `components/index.md`。

### 设计规范（生成 JSON 前必读）

| File | 说明 |
|------|------|
| `design-rules.md` | 颜色、字体、间距、圆角、布局等核心设计规则 |
| `color-variables.md` | Figma 颜色变量 Key 完整映射表 |
| `text-styles.md` | 字体样式变量 |

### 标准组件（20 个文件，使用 componentKey 调用）

| # | File | 组件 | Keywords |
|---|------|------|----------|
| 1 | `buttons.md` | Button_Primary, Secondary, Text, Trading, Group | 按钮, buy, sell, CTA |
| 2 | `navigation.md` | Navigation Bar, Tabbar | 导航栏, tabbar, logo |
| 3 | `tabs.md` | Tab (L1/L2/L3, Active/Deactive) | 标签页, tab, 切换 |
| 4 | `inputs.md` | Input_Default, Input_Compact | 输入框, input |
| 5 | `textarea.md` | Textarea | 多行输入, textarea |
| 6 | `search.md` | SearchBar | 搜索框, search |
| 7 | `dropdowns.md` | Dropdown_Text, Dropdown_Selector | 下拉菜单, dropdown |
| 8 | `selects.md` | Select_Sort, Option, List, Card | 选择列表, 单选, 多选 |
| 9 | `controls.md` | Switch, Radio, Checkbox | switch, radio, checkbox |
| 10 | `tags.md` | Tag White, Tag_Sales, Tag_Trading | 标签, 涨跌, hot |
| 11 | `dividers.md` | Divider | 分割线 |
| 12 | `date-picker.md` | Date Picker | 日期选择 |
| 13 | `stepper.md` | Progress_Number Steps, .Stepper/Unit, .Line | 步骤条, stepper |
| 14 | `slider.md` | Slider (Drag Steps) | 滑块, slider |
| 15 | `carousel.md` | Carousel (圆点), Carousel_Number (数字) | 轮播, carousel |
| 16 | `guided-tour.md` | Guided Tour | 引导, guided tour |
| 17 | `text-links.md` | Text Link, Text Instructions | 文本链接, 下划线解释 |
| 18 | `battle-chart.md` | Battle Chart | 多空对比条 |
| 19 | `toast.md` | Toast | 提示, toast |
| 20 | `alert.md` | Alert | 警告, alert, warning |

### 复合组件（5 个文件，AutoLayout 拼装 + 颜色变量绑定）

| # | File | 组件 | Keywords |
|---|------|------|----------|
| 21 | `panel.md` | Panel (Bottom Sheet) | 面板, bottom sheet, 弹出 |
| 22 | `dialog.md` | Dialog (Modal) | 弹窗, dialog, modal |
| 23 | `lists.md` | Lists | 列表, row |
| 24 | `pair-info.md` | Pair Info (Key-Value) | 键值对, label-value |
| 25 | `quick-actions.md` | Quick Actions (金刚区) | 金刚区, icon grid |

### 资源库（2 个文件）

| # | File | 说明 |
|---|------|------|
| 26 | `icons.md` | 518 icon_* + 93 BL_ 业务图标 |
| 27 | `token-logos.md` | 1289 加密货币 Token Logo |

### How to use
1. For common components (nav, buttons, inputs, dividers), use the keys from the Common Components table directly
2. For other components, identify which categories the page needs and read only those files
3. Each component file lists variant keys — use the variant key as `componentKey` in the JSON spec
4. Always pick the Dark Mode + Normal/Default variant unless the design requires otherwise
5. For composite components (Panel, Dialog, Lists, Pair Info, Quick Actions), read the file for AutoLayout assembly specs — they don't use single componentKey

---

## Override Rules

The plugin matches override keys to text layers using 4 levels (first match wins):
1. **Exact name** — override key === text layer `name`
2. **Name includes** — text layer `name` contains override key (case-insensitive)
3. **Content includes** — text layer current `characters` contains override key (case-insensitive)
4. **Parent name includes** — any ancestor's `name` contains override key

### Quick Reference — What Override Key to Use

| Component | Override Key | Replaces | Match Level |
|-----------|-------------|----------|-------------|
| **Button_Primary** | `"Primary"` | Button label | 3 (content "Primary_XL" includes "primary") |
| **Button_Secondary** | `"Secondary"` | Button label | 3 (content "Secondary_XL" includes "secondary") |
| **Button_Trading Buy** | `"Long/Buy"` | Button label | 3 (content "Long/Buy_XL" includes "long/buy") |
| **Button_Trading Sell** | `"Short/Sell"` | Button label | 3 (content "Short/Sell_XL" includes "short/sell") |
| **Button_Text** | `"Action"` | Link text | 3 (content "Action" matches) |
| **Navigation Bar title** | `"Page_Title"` | Nav title | 3 (content "Page_Title" matches) |
| **Navigation Bar right** | `"Action"` | Right action text | 3 (for Text variants only) |
| **Input_Default label** | `"Input Label"` | Field label | 1 (exact name match) |
| **Input_Default placeholder** | `"Input"` | Placeholder | 3 (content "Input" matches) |
| **Radio visible label** | `"Label"` | Radio label text | 3 (content "Label" matches) |
| **Radio title** | `"Title"` | Radio title (hidden by default) | 3 |
| **Button_Group btn1** | `"Action 1"` | Primary button (orange) | 3 (2horizontal variant) |
| **Button_Group btn2** | `"Action 2"` | Secondary button (outline) | 3 (2horizontal variant) |
| **Button_Group confirm** | `"Confirm"` | Primary button | 3 (vertical 2-btn variant) |
| **Button_Group cancel** | `"Cancel"` | Cancel button | 3 (vertical 2-btn variant) |
| **Checkbox label** | `"Label"` | Checkbox text label | 3 (content "Label" matches) |
| **Switch label** | `"Label"` | Switch text label | 3 (content "Label" matches) |
| **Dropdown_Selector placeholder** | `"Placeholder"` | Placeholder text | 3 (content matches) |
| **Dropdown_Selector selected** | `"BitListDropdown"` | Selected value text | 3 (content matches) |
| **Input_Compact placeholder** | `"Price"` | Placeholder text | 3 (default content "Price") |
| **Input_Compact suffix** | `"USDT"` | Right-side unit text | 3 (default content "USDT") |
| **Tag (all variants)** | `"Tag"` | Tag label text | 1 or 3 (layer name or content "Tag") |
| **Tag (Leaf fallback)** | `"Label"` | Leaf variant label | 1 (some Leaf variants use "Label" as layer name) |
| **Text Link** | `"Demo text"` | Link text | 3 (content "Demo text brand" includes "demo text") |
| **Text Instructions** | `"Demo text"` | Instruction text | 3 (same pattern as Text Link) |

> **Tag Override 注意：** 同时传 `"Tag"` 和 `"Label"` 两个 key 确保命中：`"overrides": {"Tag": "In Use", "Label": "In Use"}`。不同 Tag 变体的内部 text 层名称可能不同（Default 用 "Tag"，Leaf 可能用 "Label"）。

> **CRITICAL: Components with built-in labels (Checkbox, Radio, Switch) must set label text via `overrides`, NEVER by placing a separate text node next to the component.** The component already contains a "Label" text layer — override it. Placing a separate text node results in duplicate labels (e.g. "Label Funding" instead of "Funding").

---

## Design Rules (Quick Reference)

> **完整规范**: `components/design-rules.md`（9 节，Figma 验证 tokens）
> **颜色变量完整映射**: `components/color-variables.md`
> 以下仅为生成 JSON 时的快速参考，边界情况请查阅完整文档。

### 核心颜色 Tokens (Dark Mode)

**背景:**
| Token | backgroundVariable | RGB | Hex |
|-------|-------------------|-----|-----|
| Bg_Page | `f9b47b8b07c15cd8accb2b1eb6fa1cd0303457a0` | `{"r": 0, "g": 0, "b": 0}` | #000000 |
| Bg_Card_App | `f330b2b81049533f6a1627c057c12b614b41c9d4` | `{"r": 0.078, "g": 0.078, "b": 0.078}` | #141414 |
| Bg_Float | `cc4a96ef8296f4752bf28096ab31f71ebc84b096` | `{"r": 0.051, "g": 0.051, "b": 0.051}` | #0D0D0D |
| Ele_line | `8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13` | `{"r": 0.12, "g": 0.12, "b": 0.14}` | #1E1F24 |

**文字:**
| Token | colorVariable | RGB | Hex |
|-------|--------------|-----|-----|
| T1_Title | `06604334971ef5a010e564cf2fddd532e6b52e51` | `{"r": 1, "g": 1, "b": 1}` | #FFFFFF |
| T2 | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | `{"r": 0.416, "g": 0.431, "b": 0.451}` | #6A6E73 |
| T3 | `4d26a22c6de1ea23a1b7be969e3caa7ad333189e` | `{"r": 0.286, "g": 0.298, "b": 0.31}` | #494C4F |
| T4 | `b6463cecd10e51192cebc5e5d2eea3fb29ac2d30` | `{"r": 0.22, "g": 0.23, "b": 0.24}` | #383B3D |

**品牌 & 功能色:**
| Token | colorVariable | RGB | Hex |
|-------|--------------|-----|-----|
| Brand/700 | `0d73577f4ec792b83e33db70b6d3929d17925379` | `{"r": 1, "g": 0.612, "b": 0.18}` | #FF9C2E |
| Green/700 | `a7c82cc6525f151a801391642165a4bd6f3335c1` | `{"r": 0.024, "g": 0.757, "b": 0.404}` | #06C167 |
| Red/700 | `385d72216004f1730c1712f60ca4bb16dd47e235` | `{"r": 0.91, "g": 0.25, "b": 0.39}` | #E84064 |

### Page Structure Rule

```
Root (393 x H, VERTICAL, itemSpacing: 0, clipsContent: true)
  +-- StatusBar (instance, FILL) ← ONLY if no Nav Bar; Nav Bar includes StatusBar
  +-- Nav Bar (instance, FILL horizontal) ← includes built-in StatusBar
  +-- Content (frame, VERTICAL, FILL both)
  |     +-- Section 1 (FILL horizontal, HUG vertical, own padding)
  |     +-- Section 2 (FILL horizontal, HUG vertical, own padding)
  +-- Bottom Bar (Tabbar / Button_Group / sticky CTA)
```

1. **Every page must have a StatusBar.** If the page has a Nav Bar, it's built-in. If not, add a standalone StatusBar as the first child.
2. Content area = `layoutSizingHorizontal: "FILL"`, `layoutSizingVertical: "FILL"` (fills remaining height)
3. All sections inside Content = `layoutSizingHorizontal: "FILL"`, `layoutSizingVertical: "HUG"`
4. Bottom bar = last child (if present), `layoutSizingHorizontal: "FILL"`
5. Root `itemSpacing: 0` — sections manage own padding

### Layout Patterns

**Common Page Skeleton:**
```jsonc
{
  "name": "Page Name",
  "width": 393, "height": 852,
  "background": {"r": 0, "g": 0, "b": 0},
  "layoutMode": "VERTICAL", "itemSpacing": 0,
  "primaryAxisSizingMode": "FIXED", "counterAxisSizingMode": "FIXED",
  "clipsContent": true,
  "children": [
    // With Nav Bar: Nav Bar includes StatusBar, so no separate StatusBar needed
    // Without Nav Bar: add StatusBar instance as first child (see Rule 3)
    { "type": "instance", "componentKey": "9c90c04891f54f6dceb072f3ef79378ee3eb12ea",
      "name": "Navigation Bar", "layoutSizingHorizontal": "FILL",
      "overrides": { "Page_Title": "Title" } },
    { "type": "frame", "name": "Content", "layoutMode": "VERTICAL",
      "itemSpacing": 16, "paddingLeft": 20, "paddingRight": 20,
      "paddingTop": 24, "paddingBottom": 24,
      "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "FILL",
      "children": [] }
  ]
}
```

**Panel Overlay (auto height, dimmed background fills above):**
```jsonc
// Root: VERTICAL FIXED 852px. Dimmed area fills remaining, Panel hugs height.
// Panel structure: Top Area → (Handle Group + Content) → Button Area → Home Bar
// Full spec: components/panel.md
{ "type": "frame", "name": "Dimmed Background",
  "layoutMode": "VERTICAL",
  "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "FILL",
  "background": {"r": 0, "g": 0, "b": 0}, "opacity": 0.6 },
{ "type": "frame", "name": "Panel",
  "layoutMode": "VERTICAL", "itemSpacing": 0,
  "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "HUG",
  "background": {"r": 0.051, "g": 0.051, "b": 0.051},
  "backgroundVariable": "cc4a96ef8296f4752bf28096ab31f71ebc84b096",
  "topLeftRadius": 16, "topRightRadius": 16,
  "bottomLeftRadius": 0, "bottomRightRadius": 0,
  "clipsContent": true, "children": [
    // Top Area (paddingBottom:8) → Column(spacing:16) → [Handle Group, Content]
    // Handle Group (paddingTop:12, spacing:12) → [Drag Handle, Header(可选)]
    // Content (paddingH:20, paddingV:16) → 页面内容
    // Button Area / Button_Group → 底部按钮
    // .iOS_Home Bar → 底部指示条（仅手动拼接时需要）
  ] }
```

**Panel Header 可选：** 不是所有 Panel 都有标题。交易面板、说明面板等可能只有 Drag Handle 无 Header。详见 `panel.md`。

**Panel 底部按钮选型（按类型判断）：**

| 按钮类型 | 方式 | 原因 |
|---------|------|------|
| Primary / Secondary | **Button_Group** | 内置 padding + Home Bar |
| Trading Buy / Sell | **手动拼接** | Button_Group 内部是 Primary，无法替换为 Trading 样式 |

**Button_Group 在 Panel/Dialog 内 — 背景色适配（CRITICAL）：**
Button_Group 组件默认背景是 `Bg_Page` (#000000)。**当 Button_Group 放在 Panel/Dialog 等非页面容器内时，必须通过 `backgroundVariable` 覆盖背景色以匹配父容器。**

| 父容器 | Button_Group 应设置的背景 |
|--------|-------------------------|
| Page (Bg_Page #000000) | 不需要设置（默认即可） |
| Panel (Bg_Float #0D0D0D) | `"backgroundVariable": "cc4a96ef...", "background": {"r": 0.051, "g": 0.051, "b": 0.051}` |
| Dialog (Bg_Card #141414) | `"backgroundVariable": "f330b2b8...", "background": {"r": 0.078, "g": 0.078, "b": 0.078}` |

```jsonc
// Panel 内的 Button_Group（Primary/Secondary 场景）
{ "type": "instance", "componentKey": "fe01415f...(2H)",
  "layoutSizingHorizontal": "FILL",
  "backgroundVariable": "cc4a96ef8296f4752bf28096ab31f71ebc84b096",
  "background": {"r": 0.051, "g": 0.051, "b": 0.051},
  "overrides": { "Action 1": "Create", "Action 2": "Manage" } }
```

```jsonc
// Panel 内手动拼接（Trading Buy/Sell 场景）
{ "type": "frame", "name": "Button Area",
  "layoutMode": "VERTICAL", "itemSpacing": 0,
  "paddingLeft": 20, "paddingRight": 20, "paddingTop": 16, "paddingBottom": 8,
  "layoutSizingHorizontal": "FILL", "layoutSizingVertical": "HUG",
  "children": [
    { "type": "instance", "componentKey": "1786d399...(Buy XL)",
      "layoutSizingHorizontal": "FILL",
      "overrides": { "Long/Buy": "BUY BTC" } }
  ] },
{ "type": "instance", "componentKey": "f98cab63...(.iOS_Home Bar)",
  "name": ".iOS_Home Bar", "layoutSizingHorizontal": "FILL" }
```

---

## Figma File & Plugin Info

- **Component Library**: `https://www.figma.com/design/wRsLVPy2LzaNEpAlIX5FSP`

## Output

### Mode A (Direct Figma Write)
1. Build page directly in user's Figma file via `use_figma`
2. Bind all color variables and text styles
3. Print a brief layout summary showing the component instances used and node IDs

### Mode B (JSON Export)
1. Save JSON to `~/Desktop/bybit-figma-designs/{page-name}.json`
2. Copy to clipboard via `pbcopy`
3. Print a brief layout summary showing the component instances used

## Page Height Estimation

| 页面类型 | 典型高度 | 说明 |
|---------|---------|------|
| 单屏（设置、表单、结果页） | **852** | iPhone 14 一屏 |
| 中等（资产、列表、Panel host） | **1100-1400** | 1.5-2 屏 |
| 长页（Landing、综合业务页） | **1600-2000** | 按区块累加 |
| Panel（底部弹窗） | **852**（固定） | Root 固定 852，Panel 用 HUG 自适应 |

**快速估算法**: StatusBar+Nav ≈ 100, Tabbar ≈ 80, Section ≈ 120-200, 卡片行 ≈ 80-100, 按钮区 ≈ 80。累加所有区块 + 区块间 spacing。
