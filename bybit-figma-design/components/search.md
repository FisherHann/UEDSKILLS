# SearchBar

> 搜索框，带搜索图标和可选取消按钮
> 组件名: `SearchBar`
> Updated: 2026-04-03

---

## Quick Reference

| 用途 | Variant | componentKey |
|------|---------|-------------|
| 搜索栏（无取消按钮） | With Cancel=No | `c67d06d18e0cd0a953bef2e8af76c6165a2b4f04` |
| 搜索栏（带取消按钮） | With Cancel=Yes | `cd3e22afcfb31a956cb8562f430bcb32d7da89c1` |

---

## 组件规格

> Set Key: `41d0a2408fab785deff6ab0b8967434509626464`

- **变体维度**: With Cancel (Yes / No)
- **总变体数**: 2

---

## 选型指南

| 场景 | 推荐变体 |
|------|----------|
| 页面顶部搜索栏（独立搜索页） | With Cancel=Yes |
| 嵌入卡片/Section 内的搜索 | With Cancel=No |
| 列表/筛选顶部搜索 | With Cancel=No |

---

## JSON 示例

### 标准搜索栏（无取消按钮）

```jsonc
{
  "type": "instance",
  "componentKey": "c67d06d18e0cd0a953bef2e8af76c6165a2b4f04",
  "name": "SearchBar",
  "layoutSizingHorizontal": "FILL"
}
```

### 搜索页（带取消按钮）

```jsonc
{
  "type": "instance",
  "componentKey": "cd3e22afcfb31a956cb8562f430bcb32d7da89c1",
  "name": "SearchBar",
  "layoutSizingHorizontal": "FILL"
}
```
