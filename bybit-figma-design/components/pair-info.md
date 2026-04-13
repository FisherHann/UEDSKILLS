# Pair Info

> 成对信息（Key-Value 键值对，复合组件，用 AutoLayout 拼装）
> Updated: 2026-04-01

---

## AutoLayout 拼装规范

### 横向排列 (H-横向)
```
AutoLayout (horizontal, w=fill, h=auto, align=center)
  ├── Key 文字 (T2, weight=400, layoutGrow=1)
  └── Value 文字 (T1, weight=400, textAlign=right)
```
- Key 靠左，Value 靠右，一行显示
- 仅支持 S/M/L 字号

### 纵向排列 (V-纵向)
```
AutoLayout (vertical, w=auto, h=auto, gap=2)
  ├── Key 文字 (T2, weight=400)
  └── Value 文字 (T1, weight=600)
```
- Key 在上，Value 在下，gap=2（强关联）
- 支持 S/M/L/XL 字号

---

## 字号与颜色规范

| Size | Key 字号 | Value 字号 | 适用场景 |
|------|---------|-----------|---------|
| S 12 | 12px / lineHeight=18 | 12px / lineHeight=18 | 行内紧凑信息 |
| M 14 | 14px / lineHeight=22 | 14px / lineHeight=22 | 常规信息行 |
| L 16 | 16px / lineHeight=24 | 16px / lineHeight=24 | 强调信息 |
| XL 20 | 14px / lineHeight=22 | 20px / lineHeight=28 | 大数值展示（仅纵向） |

### 颜色变量

| 元素 | Token | Variable Key | Hex |
|------|-------|-------------|-----|
| Key 文字 | `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 |
| Value 文字 | `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF |

---

## 选型指南

- **横向 (H)** — 两端对齐的 label-value 行，常见于详情页、订单信息、设置项（如 "Fee ··· 0.1 USDT"）
- **纵向 (V)** — 上下堆叠的 label+数值，常见于数据卡片、资产概览（如 "Total Balance" 上方 + "$12,345" 下方）
- **XL 仅纵向** — 用于突出大数字（资产总额、核心指标），Key 保持 14px，Value 放大到 20px
- **多组并排** — 多个纵向 PairInfo 可横向排列在同一行，每组 `layoutGrow=1` 平均分布
