# Text Links

> 文字链接组件：Text Link（跳转链接）、Text Instructions（带下划线说明文字）
> Updated: 2026-04-01
>
> **与 Button_Text 的区别**:
> - **Text Link** — 跳转导航，点击后前往另一个页面/外链（如 "View All", "Learn More"）
> - **Text Instructions** — 带下划线的说明/解释文字，通常出现在正文中（如条款、免责声明链接）
> - **Button_Text** (见 buttons.md) — 触发操作/行为（如 "Cancel", "Reset", "Edit"），不跳转

---

## Quick Reference — Text Link (Dark Mode On)

带右箭头的跳转链接文字。

| Type | T1 (白色) | Brand (橙色) |
|------|----------|-------------|
| 16px | `234a0247e3e8db1a8b9140f062f8c5e6b29d08f0` | `70eb08a48e67321cba32c8d6654556cbc61a0c89` |
| 14px | `f5a15e694adec99c4db7a1d3d35be18ba1c35ccd` | `c07cbe5b3f4dab42223a33355d13a1561d216fbf` |
| 12px | `8620dad8d7dc8b772c4d09cb4981c8ab2da10441` | `eeb3bafb97f83c397eda94e8cf059242fff40e13` |
| 10px | `394096e845e8835fdc162700444294b5327489e1` | `b1e41779de20c4b536c8b244a525ca3c2c697973` |

---

## Quick Reference — Text Instructions (Dark Mode On)

带下划线的说明文字，优先使用 **Overall**（整体下划线）。

### Underline=Overall（整体下划线，推荐）

| Type | componentKey |
|------|-------------|
| 16px | `ae27197ca359a073d0b2bcec308a52e8989e8b6b` |
| 14px | `31a8fae78c6cb5831162a246b8fb71339e3a99d6` |
| 12px | `9b2d0f26f5971b3f77ddf797ad2a31a66afb2339` |
| 10px | `0232029eb4d96f0503996e6cab5729021cf2e896` |

### Underline=Only Text（仅文字下划线）

| Type | componentKey |
|------|-------------|
| 16px | `90bd7abe785d5775667af148d68b0d3efe8849dc` |
| 14px | `f504ad7e640bd4d3c2148002d6dda1504131cf0c` |
| 12px | `8c2457da6a9ed612c55aa04ff21712047e671320` |
| 10px | `ef43cd1bf674c96a1e24fff834ce828d5eb681de` |

### Underline=Off（无下划线）

| Type | componentKey |
|------|-------------|
| 16px | `f71c29d878cdb54a84eb49293d541956e4d5729a` |
| 14px | `3609a6e60b18d8db5e60571f7be11ed3ea41aea5` |
| 12px | `7d2869d50958a4c9f2e390aaf7bc7f75ed6e4dd5` |
| 10px | `bb5104791b11b8b06d61b87f29fb977d5b6dde00` |

---

## 组件规格

### 1. Text Link

> 跳转链接文字，带右箭头图标。
> Set Key: `9a7348ff4becf2c31dc2a943c8e6227ee490f59c`

- **变体维度**: Type (16px / 14px / 12px / 10px) × Colour (T1 / Brand) × Dark Mode (On / Off)
- **总变体数**: 16

### 2. Text Instructions

> 带下划线的说明/解释文字。
> Set Key: `a4db5a4ad9504a50bd23f56e292a26d612b0ccc3`

- **变体维度**: Type (16px / 14px / 12px / 10px) × Underline (Off / Only Text / Overall) × Dark Mode (On / Off)
- **总变体数**: 24

**选型指南**:
- **需要跳转到其他页面/外链** → Text Link（T1=常规白色链接，Brand=强调橙色链接）
- **正文中的说明/条款/免责链接** → Text Instructions（Overall 下划线）
- **触发操作（取消、重置、编辑）** → Button_Text（见 buttons.md）
