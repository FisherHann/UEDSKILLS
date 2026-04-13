# Stepper

> 步骤进度条：预组合步骤条、步骤单元子组件
> Updated: 2026-04-01

---

## Quick Reference — 预组合步骤条 (Dark Mode On)

直接用 `Progress_Number Steps`，按当前步骤选 key，覆盖大多数场景。

| 当前步骤 | componentKey |
|---------|-------------|
| Step 1 | `7b5cd37d50645f66d9696cffaa9c93d12a7e8618` |
| Step 2 | `8d994012198ccef92f851ee5dbb8083f45822d74` |
| Step 3 | `ec8f88bbce58fb3304a9b09d1ac7d6709349e361` |
| Step 4 | `295a35a2669cd400e119327edbb2ce4f395ebc57` |
| Step 5 | `94957c6902953a29ed6741b858e3b9f6efd7d9c4` |

---

## 子组件（自定义组合用）

需要非 1-5 步的自定义步骤条时，用以下子组件 + AutoLayout 横排拼装。

### .Stepper / Unit1（带文字标签的步骤节点, Dark Mode On）

| Step | componentKey |
|------|-------------|
| Done | `ba26a4aa8a92656dcb5bd3c8a6d4659ab61665ab` |
| Doing | `a575fb589f0506338c153029e3ab1262b199478e` |
| To Do | `8a7f890ce1f7c38bdd4f843dbdb48c07e007bb1b` |

### .Stepper / Unit2（步骤节点变体, Dark Mode On）

| Step | componentKey |
|------|-------------|
| Done/Doing | `2cbf01f17c5db520fc907ee64db502874ccabc9a` |
| Todo | `70f2c0b1bbfb1d2b199dc6f7042375e7e3f94ddd` |

### .Line（步骤间连接线, Dark Mode On）

| componentKey |
|-------------|
| `212eb3b2cdaada66b7a97d043679d05b9edda144` |

---

## 组件规格

### 1. Progress_Number Steps

> 预组合的数字步骤进度条，固定 5 步。
> Set Key: `34d77e9c66bef3972c4d873e8c5fea01cce61a20`

- **变体维度**: Step (1 / 2 / 3 / 4 / 5) × Dark Mode (On / Off)
- **总变体数**: 10

### 2. .Stepper / Unit1

> 步骤节点子组件，带文字标签。
> Set Key: `47d874241dc98b79b5b3d61a4965da3e9c869374`

- **变体维度**: Step (Done / Doing / To Do) × Dark Mode (On / Off)
- **总变体数**: 6

### 3. .Stepper / Unit2

> 步骤节点子组件变体。
> Set Key: `9247ceb773b72894da99b8de8b09ca545e96d5e0`

- **变体维度**: Step (Todo / Done/Doing) × Dark Mode (On / Off)
- **总变体数**: 4

### 4. .Line

> 步骤间连接线。
> Set Key: `1d92a2444124f2c0ba340929f6816d82b30a4048`

- **变体维度**: Dark Mode (On / Off / Dark Mode3)
- **总变体数**: 3

**选型指南**:
- **优先用 Progress_Number Steps**，直接选当前步骤即可
- 需要超过 5 步或自定义布局时，用 Unit1/Unit2 + .Line 通过 AutoLayout 横排组合
- Step 值含义：Done=已完成（勾选），Doing=进行中（高亮），To Do/Todo=待完成（灰色）
