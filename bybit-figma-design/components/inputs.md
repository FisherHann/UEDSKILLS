# Inputs

> 输入框：默认输入、紧凑输入
> Updated: 2026-04-01

---

## Quick Reference — 默认推荐 (Dark Mode Yes)

AI 生成表单页面时，大多数场景用 Default + Filled 两个状态搭配。

### Input_Default

| 用途 | Size | Status | componentKey |
|------|------|--------|-------------|
| 空输入框（大） | 48px | Default | `8f621392ad9ab4bf1e8d2db641c8211bac9dc9f7` |
| 空输入框（小） | 40px | Default | `8e2a0f43506ee0f8c01799f56f442e3d6fd9c96f` |
| 已填写（大） | 48px | Filled | `efcf85b3d5ab275d564c0d119a6bbb91791d89fc` |
| 已填写（小） | 40px | Filled | `cc5548ec1d3afe47a79a0e9010b7715a13d4b361` |
| 错误状态（大） | 48px | Error | `739efa8e0358da555f70980ead0f17cc6d4723bd` |
| 错误状态（小） | 40px | Error | `ed137920fdfbba96195a4099282d20ef0f9c38f7` |

### Input_Compact

| 用途 | Size | Status | componentKey |
|------|------|--------|-------------|
| 空输入框（大） | 48px | Default | `b4ae6e057db7b66f0411e396125d7f0d4139d7b4` |
| 空输入框（小） | 40px | Default | `a919553b56721a68d710834ef18643a9cdb7ed68` |
| 已填写（大） | 48px | Filled | `82e7fe94fd98e3c01f029014a3fb3bfc226c67b3` |
| 已填写（小） | 40px | Filled | `90fefd6a0b9467ecfcbf446907bc9ecfcda7df78` |
| 错误状态（大） | 48px | Error | `143e8c3736e6f79b5ac1613fef62080aebbebade` |
| 错误状态（小） | 40px | Error | `f2f08f7db6dd9af480d0e4bd8cc8cba208d599da` |

---

## Disable / Read Only 变体 (Dark Mode Yes)

| 组件 | Size | Status | componentKey |
|------|------|--------|-------------|
| Input_Default | 48px | Disable | `a742560c88b7f76fea60f396b10de5862f30fa94` |
| Input_Default | 48px | Read Only | `35be6ba87728873a9c1f4c210a05128363f33376` |
| Input_Default | 40px | Disable | `2b5727cef3db9c70d29bab9ab004cbe9729189af` |
| Input_Default | 40px | Read Only | `77102e32280dd8cafd1b1a9fca8bce66d5fa7a96` |
| Input_Compact | 48px | Disable | `b58f341c26d269f447d8b1f07134c8b3bb4ef615` |
| Input_Compact | 48px | Read Only | `d73604199820d6b6e561d0956ec47a3fc078fce4` |
| Input_Compact | 40px | Disable | `5a64bf6befcadc41355d01bb1b57bd9908bbddc4` |

---

## 组件规格

### 1. Input_Default

> 标准输入框，带 label 和辅助文字区域。
> Set Key: `036325f59377b245f0087d85a975860ca5a6e7ff`

- **变体维度**: Size (48px / 40px) × Status (Default / Filled / Typing / Error / Disable / Read Only) × Dark Mode (Yes / No)
- **总变体数**: 24
- **Dark Mode**: 用 **Yes/No**

### 2. Input_Compact

> 紧凑输入框，无 label 行，适合空间受限场景（如交易面板）。
> Set Key: `29cb3c6867875f10618903277d91c7ba59bf9828`

- **变体维度**: Size (48px / 40px) × Status (Default / Filled / Typing / Error / Disable / Read Only) × Dark Mode (Yes / No)
- **总变体数**: 22
- **注意**: 40px 无 Read Only 变体

**选型指南**:
- **Input_Default vs Input_Compact**: Default 有 label 区域，用于表单页面；Compact 无 label，用于交易面板等紧凑场景
- **Size**: 48px=标准表单，40px=紧凑表单
- **Status**: AI 生成常用 Default（空态）+ Filled（填写态）。Error 用于错误提示场景。Typing 是交互态，静态设计少用

---

## 附录: 交互态变体 (Typing, Dark Mode Yes)

交互态，静态设计稿一般不用。

| 组件 | Size | componentKey |
|------|------|-------------|
| Input_Default | 48px | `50a457972e5ef8f7a02d0fb0131013d9e9225699` |
| Input_Default | 40px | `d33788fed49ecb6d9b080445e8b6c12374b87968` |
| Input_Compact | 48px | `633e601d41aeb2e6beb4d51b487c2de0c42553ac` |
| Input_Compact | 40px | `74940bcfce2c255da98ec9779c503e9abc288983` |
