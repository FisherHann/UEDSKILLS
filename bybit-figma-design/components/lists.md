# Lists

> 列表组件（复合组件，用 AutoLayout 拼装）
> Updated: 2026-04-01

---

## AutoLayout 拼装规范

列表统一用 AutoLayout 拼装，灵活控制每行内容和右侧元素。

### 单行列表行
```
AutoLayout (horizontal, w=fill, h=auto, padding=[12, 20, 12, 20], gap=16, align=center)
  ├── 文字 (fill, 16px, T1 色, h=24)
  └── [可选] 右侧元素 (16×24)
```
- **行高**: 48px（内容 24px + padding 12×2）

### 双行列表行
```
AutoLayout (horizontal, w=fill, h=auto, padding=[12, 20, 12, 20], gap=16, align=center)
  ├── AutoLayout (vertical, fill, h=42)
  │     ├── 主文字 (16px, T1 色)
  │     └── 副文字 (14px, T3 色)
  └── [可选] 右侧元素 (16×24)
```
- **行高**: 66px（内容 42px + padding 12×2）

### 列表标题行
```
AutoLayout (horizontal, w=fill, h=auto, padding=[12, 20, 12, 20], align=center)
  └── 标题文字 (14px, T2 色)
```

### 完整列表组合
```
AutoLayout (vertical, gap=0, fill-width)
  ├── 列表标题行
  ├── Divider (细分割线)
  ├── 列表行
  ├── Divider
  ├── 列表行
  └── ...
```

---

## 间距参考汇总

| 参数 | 单行 | 双行 |
|------|------|------|
| 行高 | 48px | 66px |
| 内容高度 | 24px | 42px |
| Padding 上下 | 12px | 12px |
| Padding 左右 | 20px | 20px |
| 左右间距 | 16px | 16px |
| 主文字 | 16px T1 | 16px T1 |
| 副文字 | — | 14px T3 |

**右侧常见元素**: 箭头图标（跳转）、Switch（开关设置）、值文字（显示当前值）、Badge/Tag

---

## 颜色变量绑定

| 元素 | Token | Variable Key | Hex |
|------|-------|-------------|-----|
| 主文字 | `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF |
| 标题行文字 | `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 |
| 副文字 | `Gray[T]/T3` | `4d26a22c6de1ea23a1b7be969e3caa7ad333189e` | #494C4F |
