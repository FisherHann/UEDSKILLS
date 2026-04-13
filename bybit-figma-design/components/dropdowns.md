# Dropdowns

> 下拉菜单项、下拉选择器
> Updated: 2026-04-01

---

## Quick Reference — 默认推荐

### Dropdown_Text (Dark Mode On, Status=Normal, Level=T1)

下拉菜单中的单行文本项。按字号选 key。

| 用途 | Type | componentKey |
|------|------|-------------|
| 大标题项 | 18px | `9852b97e0002cc0385fdca02aefd3962026654b0` |
| 常规项 | 16px | `67dbb3d1fcb2d0c908f3322cefb8c5e89a111fa6` |
| 紧凑项 | 14px | `bcf3c1cc3ae75924e734e33a316a264e0ec0ee0b` |
| 小字项 | 12px | `857e2b02f0d62715068d92417760537dd0f01993` |
| 迷你项 | 10px | `eb54991ba57fe1c334aea09b9d1034f720184461` |

### Dropdown_Text — Selected 态 (Dark Mode On, Level=T1)

| Type | componentKey |
|------|-------------|
| 18px | `10448022ea6acde36dc76f0dee71e592893acff1` |
| 16px | `384d28228ddf46f0fa1f18c3d745026ac09a55c9` |
| 14px | `5ac0be4f226f3470b8e3bf1fa1fb1a1ab4147f98` |
| 12px | `498e10429f6e6dc5249686c66d5143e565ca7a3e` |
| 10px | `76e014a24126171a8f69d53c39f7d66ff5cc1762` |

### Dropdown_Selector (Dark Mode Yes)

下拉选择器触发器。

| 用途 | Size | Status | componentKey |
|------|------|--------|-------------|
| 空选择器（大） | L 48px | Placeholder | `548ec1a5a9694e10a6c21d711717dc56823f3d88` |
| 空选择器（中） | M 40px | Placeholder | `fc9d1b835a36d40775cd8ac12830fdc15241a7ab` |
| 空选择器（小） | S 32px | Placeholder | `a853b742349e8bffb3065c6ed4cce597afe28511` |
| 空选择器（迷你） | XS 24px | Placeholder | `98a9faa23e7ed5a52e891600842df101b01c5867` |
| 已选择（大） | L 48px | Selected | `f6265c736871fcc5754511b06ec80e2db1544dcd` |
| 已选择（中） | M 40px | Selected | `400e5005b9f1523e0bc0fef85698c54c2510d075` |
| 已选择（小） | S 32px | Selected | `a62506c62485e5765390f6d85a32dd87c5143c47` |
| 已选择（迷你） | XS 24px | Selected | `9179ae0ea0e84583561afd89c50b086f568ca86b` |

---

## Disable 变体

### Dropdown_Text (Dark Mode On, Level=T1)

| Type | componentKey |
|------|-------------|
| 18px | `fc271e0ce95015d785a248344adb51aee9560b98` |
| 16px | `1f6e483bab66c1e4b3ed6d61bf5c79f7f0cc09c0` |
| 14px | `f026b9de983a5d2f5fffdea48c0bc30e83d408a5` |
| 12px | `1a8e60751cf4a87baa7fe82892130a3d65b9ecf5` |
| 10px | `e552f44088e762444ff7e9c391851c82a8f918b3` |

### Dropdown_Selector (Dark Mode Yes)

| Size | componentKey |
|------|-------------|
| L 48px | `b2688b0debd0ab06e9d9fc28551dcab3533d2c92` |
| M 40px | `11021150c14c0406fea05c6a7be4e462ceebd474` |
| S 32px | `a7b82fdc49c647f572469d18f3ebdd3b4c2bd358` |
| XS 24px | `acc235735797d27a1ee955d179e4803a9dae2fba` |

---

## 组件规格

### 1. Dropdown_Text

> 下拉菜单中的文本行项目，可标记选中态。
> Set Key: `0a3c111c67a0051e0ce2a2409ab9d7d707db0841`

- **变体维度**: Dark Mode (On / Off) × Type (18px / 16px / 14px / 12px / 10px) × Status (Normal / Selected / Disable) × Level (T1 / T2 / T3)
- **总变体数**: 90
- **Dark Mode**: 用 **On/Off**

### 2. Dropdown_Selector

> 下拉选择器触发器，点击展开下拉菜单。
> Set Key: `ee7456ba5d8a305f458ed5037ab728255c22694d`

- **变体维度**: Size (L 48px / M 40px / S 32px / XS 24px) × Status (Placeholder / Selected / Focus / Disable) × Dark Mode (Yes / No)
- **总变体数**: 32
- **Dark Mode**: 用 **Yes/No**

**选型指南**:
- **Type (Dropdown_Text)**: 字号，跟随上下文排版。16px 最常用
- **Level**: T1=主文字色，T2=次级文字色，T3=辅助文字色。默认用 T1
- **Status**: Normal=未选中项，Selected=已选中项（带品牌色/勾选），Disable=不可选
- **Dropdown_Selector Size**: L=表单场景，M=紧凑场景，S/XS=行内筛选

---

## 附录 A: Dropdown_Text 其他 Level (Dark Mode On, Normal)

| Type | T2 | T3 |
|------|----|----|
| 18px | `54931c55c9da4706d4aeebd20f870a2faaabf188` | `9282f9b8194ee012d93824278b5dadf243ea71c7` |
| 16px | `7b7dc945c3e84ab59ffb60f1fb407686ba499855` | `5223900ef660f578724cd0d39ef4e8b42011c904` |
| 14px | `c0e04020bfb2280477b520fe9059d0249c0c5be2` | `a02247943cc504f597110a129319bede4900847d` |
| 12px | `c7a23e080ef6fb9f9d1bc0d3099713f69be73dc4` | `946673693802d0d32bfa5cdbd4d191848e5163e6` |
| 10px | `19b6e35b6b2a0ad7f69e8bd3fb1be7d40daa9211` | `0d1eab347aaf269562f068ee5b47e096059443c1` |

## 附录 B: Dropdown_Selector Focus 态 (Dark Mode Yes)

交互态，静态设计一般不用。

| Size | componentKey |
|------|-------------|
| L 48px | `aa64441d82929dca861e59d67327d7b3659a1191` |
| M 40px | `5f41d54232606032a992bb1490ec254e4642562c` |
| S 32px | `f1f50fda7793737802a50f247b5b212591c11d6f` |
| XS 24px | `73673a449623cfd8b4b607a03bc91d8251eec85d` |

## 附录 C: Dropdown_Text 完整交叉表 (Dark Mode On, Selected/Disable × T2/T3)

<details>
<summary>展开完整列表</summary>

**Selected, T2:**
18px `fa3977af7d8b9ba1bc75319065d2971605279eb1` | 16px `232d9644109e78f27074645cd22e0a9e64b5be5f` | 14px `bbe4a85832f42808130d87a83ce84447bd6142e3` | 12px `3df2497b03a4bee47d230ea4cb4ce33dddd76bef` | 10px `62728023a757cc77b77deec6cbc372ef2e061725`

**Selected, T3:**
18px `1ae74d744207e04d308bbd1fae681ec16a58dcfd` | 16px `afe11397bd60400565b90c82b7585304b60eb05b` | 14px `6ac02e48029a2049991fe820e910fb6835636233` | 12px `734c45d074225f9bf2e219112e415ff161f384d9` | 10px `d2da61af9a90083e3b63a09a1b2ddea324c2892b`

**Disable, T2:**
18px `033dd4b0c514b8c36ec79036ababd9aefc43db4d` | 16px `33920428e21c6ab097bf9914c7ca26af88187392` | 14px `4934d08d028356c2399480a707b9d8b8c85a03e8` | 12px `2773ddc8e78b3fd270d0779d97fac499a96f7c68` | 10px `4d060364677cbc54d9c9d45d3f2daf32c28b6d97`

**Disable, T3:**
18px `1eccb2beb2a9be2132280057efb3f30d77592524` | 16px `caeb18f717f6be1c13fee0fe4534691f39fcbc9c` | 14px `a2f84908ec26ee9424f0327ad07037cfc2fd5622` | 12px `5645d3be5d04abae8fee1fcf43eb9de02491d847` | 10px `154dd6a8071bdef1fa73334ba2fd10368a3f41a9`

</details>
