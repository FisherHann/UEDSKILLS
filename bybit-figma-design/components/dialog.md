# Dialog (Modal)

> 居中对话框（复合组件，用 AutoLayout 拼装）
> Updated: 2026-04-01
> **与 Panel 的区别**: Dialog 居中浮层，用于确认/提示/营销弹窗；Panel 底部弹出，用于任务/选择/说明。

---

## AutoLayout 拼装规范

### 页面级结构

```
Root (393×852, vertical, align=center, justify=center, bg=#000, opacity 遮罩)
  └── Dialog Container
```

### Dialog 结构

```
AutoLayout (vertical, w=303, h=auto, bg=Bg_Float, cornerRadius=16, padding=[32, 24, 24, 24], gap=16, align=center, clipsContent=true)
  ├── Result 图标 (状态大图标，居中)
  ├── 标题文字 (18px, weight=600, T1, center)
  ├── 描述文字 (14px, weight=400, T2, center)
  └── 按钮区域
        ├── [单按钮] Button_Primary L, layoutAlign=STRETCH
        └── [双按钮] horizontal, gap=8
              ├── Button_Secondary L (layoutGrow=1)
              └── Button_Primary L (layoutGrow=1)
```

### Result 图标组件

操作结果/状态提示的大图标，**必须放在标题上方**，用于强化视觉反馈（成功✓、警告⚠、错误✗等）。

- **componentKey (Dark Mode On)**: `fa9a9bb6d16f96a5b21ba5d8dcd06914cb31f9b9`
- Set Key: `f1a0638b59c3c464182b4ae058ac5cfa75e82190`
- 变体: Dark Mode (On / Off)，共 2 个

---

## 关键尺寸

| 参数 | 值 |
|------|-----|
| 宽度 | 303px |
| 圆角 | 16px（四角） |
| 背景 | Bg_Float (#0D0D0D) |
| paddingTop | 32 |
| paddingBottom | 24 |
| paddingLeft/Right | 24 |
| 内部 gap | 16 |
| 标题字号 | 18px / weight=600 |
| 描述字号 | 14px / weight=400 |
| 按钮高度 | L (40px) |

---

## 遮罩层

Dialog 浮层覆盖在暗色遮罩上：

```
Root (393×852, bg=#000)
  ├── 遮罩背景 (fill, bg=#000, opacity=0.6)
  └── Dialog (居中定位)
```

实现方式：Root 设置 `primaryAxisAlignItems: "CENTER"`, `counterAxisAlignItems: "CENTER"`，Dialog 自动居中。

---

## 颜色变量绑定

| 元素 | Token | Variable Key | Hex |
|------|-------|-------------|-----|
| Dialog 背景 | `Gray[Bg]/Bg_Float` | `cc4a96ef8296f4752bf28096ab31f71ebc84b096` | #0D0D0D |
| 标题 | `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF |
| 描述 | `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 |

---

## 组件规格

### Dialog

> Set Key: `ab60d0b7257e807f8fa41bb98c8a1ebf3e4266b5`

- **变体维度**: Dark (Yes / No) × Type (System Dialog / Promotional Dialog)
- **总变体数**: 4
- **Dark Mode**: 属性名 `Dark`，值为 **Yes / No**
- **componentKey (Dark Yes, System Dialog)**: `8779c7d62650922e82f64813d22a41c60b823ddd`

### Result

> Set Key: `f1a0638b59c3c464182b4ae058ac5cfa75e82190`

- **变体维度**: Dark Mode (On / Off)
- **总变体数**: 2
- **componentKey (Dark Mode On)**: `fa9a9bb6d16f96a5b21ba5d8dcd06914cb31f9b9`

---

## 设计检查清单

1. **Result 图标必须有** — 始终作为 Dialog 第一个子元素，不可省略
2. **宽度固定 303px** — 不是 fill，不是 393
3. **背景 Bg_Float (#0D0D0D)** — 与 Panel 相同
4. **四角均 16px 圆角** — 与 Panel 不同（Panel 仅顶部圆角）
5. **paddingTop=32** — 比侧面/底部的 24 更大，给顶部更多呼吸空间
6. **按钮用 L 尺寸 (40px)** — 不是 XL，Dialog 空间有限
7. **双按钮**: 左 Secondary 右 Primary，水平排列，各 `layoutGrow=1`，间距 8
8. **所有文字居中对齐** — 标题和描述都是 `textAlignHorizontal: "CENTER"`
9. **Dialog 不需要 iOS Home Bar** — 仅 Panel 和全屏页需要
