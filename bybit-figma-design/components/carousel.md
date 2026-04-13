# Carousel

> 轮播指示器：圆点式、数字式
> Updated: 2026-04-01

---

## Quick Reference — Carousel 圆点指示器 (Dark Mode On)

按页数选 Nodes，按当前页选 Highlight，默认用 small。

### Nodes=2

| Highlight | small | medium |
|-----------|-------|--------|
| 1st | `b910d336ef97fd9b712abcd359986bba0e9bedbb` | `112bd14e5936075c8909cc553a23f112d8606e6c` |
| 2nd | `9659f7c32db0c37e9eb6028dcc2a76ecf81b8956` | `fb65bcd7481c53c27e64bab9d59448ba705da25a` |

### Nodes=3

| Highlight | small | medium |
|-----------|-------|--------|
| 1st | `ab383229798edc6fe440abd8a9737a006442d44f` | `aa67ea6a9e97198ad01d4f6b32585e1d0154bfc4` |
| 2nd | `c9365ea86b3ebbecdc413b4d5b44aeaa5738be16` | `7464ed900871a11fe035cb38fb5eaf998c30777d` |
| 3rd | `00cfc5e7ec98d4bedef07f27345728b2981a04c8` | `da4aadf3bdc4b204c5e5624a8c66561d63737338` |

### Nodes=4

| Highlight | small | medium |
|-----------|-------|--------|
| 1st | `72b65906e7e1fca53da11c04a2a6e05c07dbefd9` | `b45fa58fb64a69fd7614426ead1518bdb91035a5` |
| 2nd | `c92cca06796d498fac301e69ef661df80a5eae02` | `23d5842561229c17b85b408e9d480d58462f95bf` |
| 3rd | `c839a5fd603b03b895377311ec24222a035d2754` | `6ff8293599468b0727f1736d66c52fa4ea37b395` |
| 4th | `235dcd6abc5cc46b77d5128ad65c9e52499eb65a` | `f7c2e3bc8f639b8ea8dd4b244a74d548faabc0ea` |

---

## Quick Reference — Carousel_Number 数字指示器 (Dark Mode On)

显示 "当前页/总页数"（如 1/5），按总页数选 Step，按当前页选 Node。

| Step\Node | 1 | 2 | 3 | 4 | 5 |
|-----------|---|---|---|---|---|
| **2** | `00c3171ca718be447608ccbd55dd45d8a0c2b183` | `5acb6b44eb8cc1dd886de20f19d8d416d6638f25` | — | — | — |
| **3** | `d38043880775887cde9c5e2142a5d0c9a69f7931` | `18c8f31a8e96326b70521b8a8976ba18b021fbed` | `bb0b1d9b9bad2ed3f99ed1028713465c9216a046` | — | — |
| **4** | `0915ad60cb918ee2428abc70c69c238cb3525e76` | `837f66a9be1bf89946bb368c5c7d223a58f953b1` | `477bc8f501c02d4791ac26a559ac6d7b3f776668` | `83ee89f0c64fc6b8b6e40c1a8e509df00484cfce` | — |
| **5** | `6d0341291211e635a10bcb356fd7817437bce613` | `474318dad2670cacc461fe1f2d79de65f56acfa4` | `79d563d8f9f928d40522c7dc936e27494d224dfc` | `f8ad853e9396ad63b83899cc6c5529fde9fa9d8b` | `d1aaa69247cce0c1643c503cbda01901c006e28e` |

---

## 组件规格

### 1. Carousel

> 圆点轮播指示器。
> Set Key: `a31a30c3a72d5e2e10d3b4e5b395e1678b24d5e4`

- **变体维度**: Dark Mode (On / off) × size (small / medium) × Nodes (2 / 3 / 4) × Highlight (1st–4th)
- **总变体数**: 36
- **Dark Mode**: 值为 **On / off**（注意 off 小写）
- **约束**: Highlight ≤ Nodes（如 Nodes=2 只有 1st/2nd）

### 2. Carousel_Number

> 数字轮播指示器（显示 "当前/总数"）。
> Set Key: `c2f00703912731180e775120fd04d0ba9bdf7fac`

- **变体维度**: Dark Mode (On / Off) × Step (2 / 3 / 4 / 5) × Node (1–5)
- **总变体数**: 28
- **Dark Mode**: 值为 **On / Off**
- **约束**: Node ≤ Step（如 Step=3 只有 Node 1/2/3）

**选型指南**:
- **圆点式 (Carousel)** — 图片轮播、Banner 轮播，视觉轻量
- **数字式 (Carousel_Number)** — 步骤引导、教程页，需要明确显示进度
- **size**: small=常规场景，medium=大 Banner 底部
- **Nodes/Step**: 总页数；**Highlight/Node**: 当前高亮页
