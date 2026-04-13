# Alert

> 内联提示条（嵌入页面/卡片内的提示信息）
> Updated: 2026-04-01

---

## Quick Reference (Dark Mode On)

| Type | Short | Long |
|------|-------|------|
| Warning | `8792ab1b10deb058f070e76e78f76dbacdc93981` | `d727d92776f2bb7537f0ab8f225d5f770e17e50a` |
| Error | `9d844da95e31975b98ec1f600599e3012874a1fa` | `611dc9263125b25cb17439a23bb1a83673253355` |
| Success | `42a115b4d28f19a696b3abd2b1eccc9bf6fe6340` | `93c508e423c891f079920d13e722caaea6eb5141` |
| Neutral | `11f080ee7ff2906e553ffab8a221e0a298ec1767` | `f591628ee5ac79f0717e687af75a0d8ab671b85b` |
| No Background | `ddb5d38a4922d9487e19afa5f37b1d15fbba5a8d` | `687da8efa404dccb9c6f4f0a9cac52c894ac7bc1` |

---

## 组件规格

> Set Key: `816a9b1b521013cf25f0840b00b89d1e9b1c9c6c`

- **变体维度**: Type (Warning / Error / Success / Neutral / No Background) × Length (Short / Long) × Dark Mode (On / Off)
- **总变体数**: 20

**选型指南**:
- **Type**: Warning=风险提醒（黄），Error=错误/危险（红），Success=成功确认（绿），Neutral=中性信息，No Background=无底色纯文字提示
- **Length**: Short=单行简短提示，Long=多行详细说明
