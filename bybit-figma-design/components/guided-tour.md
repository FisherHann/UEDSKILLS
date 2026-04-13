# Guided Tour

> 新手引导气泡提示，带箭头指向目标元素
> Updated: 2026-04-01

---

## Quick Reference — 默认推荐 (Dark Mode Yes)

按箭头方向选 key。Arrow 表示箭头指向的位置（即气泡相对于目标元素的朝向）。

### Size=Default（最常用）

| Arrow | componentKey |
|-------|-------------|
| Up left | `a4657b99ed901379f1584bb012211ef615330923` |
| Up center | `a73a34dc507448440689108262abeeb472373ea6` |
| Up right | `d59ea0c97bc4afbf22f6c807d7f3c36a8ea76c6d` |
| Down left | `f92895092dad6aedb7f68d0de0d29461a3b3cdf4` |
| Down center | `0cac5e1b2dd1db0fbdbdb68b5fa816286393db4a` |
| Down right | `85a96ab075c46e3278b98a05ce8f797f7b68257a` |

### Size=Min（紧凑场景）

| Arrow | componentKey |
|-------|-------------|
| Up left | `8145b6b656a4f0f06332cfc449bfc5e8a195fe4b` |
| Up center | `c1275375b9a011bfede165ba989919b20c650926` |
| Up right | `878acaf35fc3a716fe885f2ba3ce228779c16a9e` |
| Down left | `08e1478134ebc652eb289e1cfbf0704e297b84d5` |
| Down center | `c7eff40b576aba77630ada87ff3781af5b0e2b5d` |
| Down right | `0f9b19bae8a80cdd255a3a00e90c0a5aab3d12da` |

### Size=Max（大段说明）

| Arrow | componentKey |
|-------|-------------|
| Up left | `9cf6e63c36c9aa7a9301c4190db777b2819708d2` |
| Up center | `6b0835643f7953df3328f29cbc1a953863835733` |
| Up right | `9d192961a3aa8327dd5a78498425a2a8f1eefd2d` |
| Down left | `0727575193298248def8e0f0e5cd6051782a5420` |
| Down center | `fc78ac6881a6ab74297d3714e560cfdd01fe1d69` |
| Down right | `6860b2bc890f1c15b6bf30e1dd5aa8c1b01c32ae` |

---

## 组件规格

> Set Key: `a510b1a052d81004de5a365c1c82793fbfbda4a9`

- **变体维度**: Size (Min / Default / Max) × Arrow (Up left / Down left / Up center / Down center / Up right / Down right) × Dark Mode (Yes / No)
- **总变体数**: 36
- **Dark Mode**: 用 **Yes/No**（非 On/Off）

**选型指南**:
- **Size**: Min=短提示文字，Default=常规说明（最常用），Max=长段落说明
- **Arrow**: 根据目标元素在屏幕中的位置选择。Up=箭头朝上（气泡在目标下方），Down=箭头朝下（气泡在目标上方）。left/center/right=箭头水平位置
