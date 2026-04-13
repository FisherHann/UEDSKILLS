# Navigation

> 顶部导航栏、底部 Tabbar
> Updated: 2026-04-01

---

## Quick Reference — 默认推荐 (Dark Mode On)

### Navigation Bar

| 用途 | Left | Right | componentKey |
|------|------|-------|-------------|
| 常规页面（返回+右侧文字） | Arrow | Text | `9c90c04891f54f6dceb072f3ef79378ee3eb12ea` |
| 常规页面（返回+右侧图标） | Arrow | Icons | `38d96d007ec5b7d3623ae2c4f46e62b30842e859` |
| 取消操作（取消+右侧文字） | Cancel | Text | `3dc2c3f2ff78cc208fda10e159e6621f048df1d3` |
| 取消操作（取消+右侧图标） | Cancel | Icons | `bed063675e4020d86c3f1252acc27a2c7a5bb829` |
| 弹窗/面板（关闭+右侧文字） | Close | Text | `00bc2ac24a39497ca5e1e5be70f575869253195d` |
| 弹窗/面板（关闭+右侧图标） | Close | Icons | `5393a7e1259981b38abde19e1af733044e375625` |

### Tabbar

| 当前页 | componentKey |
|--------|-------------|
| Home | `6feb5e098101edce2fc2b01967d3dc308babe5b3` |
| Markets | `0a8938c4b46e0689052ad15552da38366dff7897` |
| Trade | `8fe040712a23c9ff0861633361584224172e1e26` |
| Earn | `4a56a2bbd484e1784ce35d557e2ddf4f3551e356` |
| Assets | `e1054e764e9957c8246e1db719f21ea79b70d1d0` |

---

## 组件规格

### Navigation Bar

> Set Key: `613bb90ace7851401ae1bbff61f43790d96f3711`

- **变体维度**: Left (Arrow / Cancel / Close) × Right (Text / Icons) × Dark Mode (On / Off)
- **总变体数**: 12

**选型指南**:
- **Left**: Arrow=返回上一页（最常用），Cancel=取消当前操作，Close=关闭弹窗/面板
- **Right**: Text=右侧放文字操作（如"完成"、"保存"），Icons=右侧放图标按钮

### Tabbar

> Set Key: `7979c9ee6cf2b631d3eafd0d0b2b601bc78aff7b`

- **变体维度**: Selection (Home / Markets / Trade / Earn / Assets) × Dark Mode (On / Off)
- **总变体数**: 10

**选型指南**:
- **Selection**: 当前激活的 Tab 页。根据设计稿所属页面选择对应 Tab。
