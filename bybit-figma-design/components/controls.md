# Controls

> 控件：开关、单选、复选
> Updated: 2026-04-01

---

## Quick Reference — 默认推荐 (Dark Yes)

### Switch

| 用途 | Size | Turn On | componentKey |
|------|------|---------|-------------|
| 大开关-关 | L-20px | No | `c151681df79878443ab92f0134f19cf3f55e5ecb` |
| 大开关-开 | L-20px | Yes | `1acc910f0242ffd50cfe5eae3f07ef04c8df973d` |
| 小开关-关 | S-16px | No | `86bc77879b458cf0057418114db230d9e6f6c38a` |
| 小开关-开 | S-16px | Yes | `bc4d78fe85a08f85789dc4f0a8be29bfa0718995` |

### Radio

| 用途 | Checked | componentKey |
|------|---------|-------------|
| 未选中 | No | `a06e259122d7d63a6317202996fd4c449641c1fe` |
| 已选中 | Yes | `6cd457baf7f5ba6ce186fb37ebbc90af74d6dddb` |

### Checkbox

| 用途 | Checked | componentKey |
|------|---------|-------------|
| 未选中 | No | `8a1cb693d9617f67fdc1f2e6025a5656bf1d3bf1` |
| 已选中 | Yes | `21e3d58494dbc1b4e3ff19e1f7444cfb62208ee5` |

---

## Disabled 变体 (Dark Yes)

### Switch

| Size | Turn On | componentKey |
|------|---------|-------------|
| L-20px | No | `1ccb6a1dfa957f7e568b9959d334c9761abb256e` |
| L-20px | Yes | `739dae7c9e1b40b3cdbfdf20e00a7fbe0713a0d2` |
| S-16px | No | `fd4fe3c64115ea15db90bcec0c0a4ec98441b8ba` |
| S-16px | Yes | `b6fd555a8c0c9ddbb7a456c44d016d54b13e46aa` |

### Radio

| Checked | componentKey |
|---------|-------------|
| No | `bd7da7c4ee11fc62b26ef35fbce5201e860a105d` |
| Yes | `3a8e9bbdaa143c129b97f3d0b7def5938f689e48` |

### Checkbox

| Checked | componentKey |
|---------|-------------|
| No | `224341a7f5e758f507b7cedb6c1cdad46d458843` |
| Yes | `5724bf860492b100f6d8a68e1c06530b8f443316` |

---

## 组件规格

### 1. Switch

> 开关控件。
> Set Key: `d770bc0c7d73ad6283d529e75dd156945fb0f8e5`

- **变体维度**: Dark (Yes / No) × Size (L-20px / S-16px) × States (Normal / Disabled) × Turn On (Yes / No)
- **总变体数**: 16
- **选型**: L-20px=常规场景，S-16px=紧凑场景（如列表行内）

### 2. Radio

> 单选圆形控件。
> Set Key: `a784a90e8526124628697a344705f0b11059f36f`

- **变体维度**: State (Normal / Disabled) × Checked (Yes / No) × Dark (Yes / No)
- **总变体数**: 8

### 3. Checkbox

> 复选方形控件。
> Set Key: `6a328f08bc3e31b42ff3f6cadbce6194ecee955e`

- **变体维度**: State (Normal / Disabled) × Checked (Yes / No) × Dark (Yes / No)
- **总变体数**: 8

**通用说明**: 三个控件的 Dark Mode 属性名均为 `Dark`，值为 **Yes/No**
