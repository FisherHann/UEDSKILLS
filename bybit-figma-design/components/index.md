# Bybit Design V2 — Component Library Index

> 组件库索引，所有组件仅保留 Dark Mode 变体
> Updated: 2026-04-01

---

## 设计规范

| 文件 | 说明 |
|------|------|
| [design-rules.md](design-rules.md) | 颜色、字体、间距、圆角、布局等核心设计规则 |
| [color-variables.md](color-variables.md) | Figma 颜色变量 Key 完整映射表 |
| [text-styles.md](text-styles.md) | 字体样式变量 |

---

## 标准组件（使用 componentKey 调用）

| 文件 | 组件 | 变体数 |
|------|------|--------|
| [buttons.md](buttons.md) | Button_Primary, Button_Secondary, Button_Text, Button_Trading, Button_Group | 144 |
| [navigation.md](navigation.md) | Navigation Bar, Tabbar | 22 |
| [inputs.md](inputs.md) | Input_Default, Input_Compact | 46 |
| [textarea.md](textarea.md) | Textarea | 10 |
| [search.md](search.md) | Search | 10 |
| [dropdowns.md](dropdowns.md) | Dropdown_Text, Dropdown_Selector | 122 |
| [selects.md](selects.md) | Select_Sort, Select_Option, Select_List, Select_Card | 98 |
| [controls.md](controls.md) | Switch, Radio, Checkbox | 32 |
| [tags.md](tags.md) | 默认 Tag (White_Normal/Light), Tag_Sales, Tag_Trading | 80 |
| [dividers.md](dividers.md) | Divider | 16 |
| [date-picker.md](date-picker.md) | Date Picker | 2 |
| [stepper.md](stepper.md) | Progress_Number Steps, .Stepper/Unit, .Line | 23 |
| [slider.md](slider.md) | Slider (Drag Steps) | 12 |
| [carousel.md](carousel.md) | Carousel (圆点), Carousel_Number (数字) | 64 |
| [guided-tour.md](guided-tour.md) | Guided Tour | 36 |
| [text-links.md](text-links.md) | Text Link, Text Instructions | 40 |
| [battle-chart.md](battle-chart.md) | Battle Chart (多空对比条) | 12 |
| [toast.md](toast.md) | Toast | 10 |
| [alert.md](alert.md) | Alert | 20 |

---

## 复合组件（用 AutoLayout 拼装，含颜色变量绑定）

| 文件 | 组件 | 说明 |
|------|------|------|
| [panel.md](panel.md) | Panel (Bottom Sheet) | 底部弹出面板，含 Drag Handle/Button_Group key |
| [dialog.md](dialog.md) | Dialog (Modal) | 居中对话框，含 Result 图标 key |
| [lists.md](lists.md) | Lists | 列表行间距参考规范 |
| [pair-info.md](pair-info.md) | Pair Info | Key-Value 键值对 |
| [quick-actions.md](quick-actions.md) | Quick Actions (金刚区) | 图标网格 |

---

## 资源库

| 文件 | 说明 |
|------|------|
| [icons.md](icons.md) | BL_ 业务图标库 (componentKey 索引) |
| [token-logos.md](token-logos.md) | 加密货币 Token Logo 库 |
