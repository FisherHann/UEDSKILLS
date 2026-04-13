# Toast

> 轻提示（操作反馈浮层）
> Updated: 2026-04-01

---

## Quick Reference (dark on)

| state | componentKey |
|-------|-------------|
| successful | `584267353c3e214e95788273cfad282b889af6b2` |
| Error or failure | `4104ee3dfd98bbe38d08bbda631da1c9ccf7a0a5` |
| Reminder | `21f2a9ff8742cb7a04613a9b50a62c360995dd85` |
| text | `f033adf908f1457c40b92254b815a89653cdc774` |
| loading | `d11f212eaf3363f196bcfa738804fbd7020c1aae` |

---

## 组件规格

> Set Key: `5afb4ea9020647d41b3494165a039c38774bd58a`

- **变体维度**: dark (on / off) × state (successful / Error or failure / Reminder / loading / text)
- **总变体数**: 10
- **Dark Mode**: 属性名 `dark`，值为小写 **on / off**

**选型指南**:
- **successful** — 操作成功（提交、保存、转账完成）
- **Error or failure** — 操作失败、网络错误
- **Reminder** — 提醒/警告（非错误，需注意）
- **text** — 纯文字提示（中性信息）
- **loading** — 加载中状态
