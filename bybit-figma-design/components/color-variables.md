# Color Variables — Figma Library Key Map

> Scanned from Bybit component library (2026-03-24).
> Use `colorVariable` / `backgroundVariable` in JSON spec to bind Figma color variables.
> Plugin falls back to `color` / `background` RGB if variable import fails.

---

## Text Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Gray[T]/T1_Title` | `06604334971ef5a010e564cf2fddd532e6b52e51` | #FFFFFF | Primary text, titles, values, active states |
| `Gray[T]/T2` | `c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955` | #6A6E73 | Secondary text, labels, descriptions, inactive tabs |
| `Gray[T]/T3` | `4d26a22c6de1ea23a1b7be969e3caa7ad333189e` | #494C4F | Tertiary text, hints, timestamps, icon hints |
| `Gray[T]/T4_Dis` | `b6463cecd10e51192cebc5e5d2eea3fb29ac2d30` | #383B3D | Disabled text |

## Brand Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Brand/700-normal` | `0d73577f4ec792b83e33db70b6d3929d17925379` | #FF9C2E | CTA fill, active indicators, brand links |
| `Brand/900-text` | `db842d9c60d095c8eb615857fd8b2f72cd392b7c` | #F29229 | Brand text on dark bg |
| `Brand/600-hover` | `3c73b090e8f587961212a8a5c70f2bad7dd65987` | — | Hover state |
| `Brand/800-pressed` | `1f575f2ddccc99678f15eb6c544aed35b885d965` | — | Pressed state |
| `Brand/100-bg` | `89c85b174c59a7742d8ba12ad0f9de3d01ddb076` | — | Brand tint background |

## Green (Buy/Success)

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Green/700-normal` | `a7c82cc6525f151a801391642165a4bd6f3335c1` | #06C167 | Buy/Long, positive, success, bid |
| `Green/600-hover` | `ac50a9c648c94e51b81b4993d109eff0ffa0ca52` | — | Hover state |
| `Green/800-pressed` | `7ac07b62576ad6408ee91c743fee271cf1353912` | — | Pressed state |
| `Green/100-bg` | `bff33af5db5533e1c48d2e816f00c19dda01c6a9` | — | Green tint background |

## Red (Sell/Error)

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Red/700-normal` | `385d72216004f1730c1712f60ca4bb16dd47e235` | #E84064 | Sell/Short, negative, error, ask |
| `Red/600-hover` | `548654bc155b9f0f0259b25a7e0936e2f397c90d` | — | Hover state |
| `Red/800-pressed` | `b44c37d1ff55af8c5d4ec33aca087d894a728f66` | — | Pressed state |
| `Red/100-bg` | `9f88451a2cf3da2349004c708d3c28271b8ccb78` | — | Red tint background |

## Background Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Gray[Bg]/Bg_Page_App` | `f9b47b8b07c15cd8accb2b1eb6fa1cd0303457a0` | #000000 | Page background (App) |
| `Gray[Bg]/Bg_Card_App` | `f330b2b81049533f6a1627c057c12b614b41c9d4` | #141414 | Card surface (App) |
| `Gray[Bg]/Bg_Page_Web` | `2e1f146922c721fd4cae7628612e578caa63ef36` | — | Page background (Web) |
| `Gray[Bg]/Bg_Card_Web` | `db46c8d32aa87a158556e8c347ac051507b3fe86` | — | Card surface (Web) |
| `Gray[Bg]/Bg_Float` | `cc4a96ef8296f4752bf28096ab31f71ebc84b096` | — | Float/popup background |

## Element Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `Gray[Ele]/Ele_line` | `8d6cfec0b0e41f7c2bbcf14083aacb9f7720cf13` | #1E1F24 | Interactive surfaces (toggles, chips) |
| `Gray[Ele]/Ele_border` | `3d674302b69c044d756f796491c084fe4eaf7a78` | — | Border color |

## Static Colors

| Token | Variable Key | Hex | Use for |
|-------|-------------|-----|---------|
| `static_White` | `bda6981840f965962231e8c565d930e9b0a3a543` | #FFFFFF | Text on colored bg |
| `static_Black` | `7d48adfccbfde60e7377b5eaf6deb509ad472740` | #121214 | Text on brand orange bg |
| `static_popup` | `92a537b93f1daab41a67104ee1b2167080474fe4` | — | Popup overlay bg |

## Utility

| Token | Variable Key | Use for |
|-------|-------------|---------|
| `Trans_Hover` | `6eb232781fe44103f00782622231169827cdd0c7` | Transparent hover overlay |
| `Trans_Mask` | `4441b6bcd3ab4e18593261812be40976fe39e17f` | Mask overlay |

---

## JSON Spec Usage

### Text node — bind text color variable
```jsonc
{
  "type": "text",
  "content": "Total Balance",
  "colorVariable": "c96bbd1bcf98d1ca78e4f6e08a59a76c60de1955",
  "color": {"r": 0.416, "g": 0.431, "b": 0.451}
}
```

### Frame node — bind background variable
```jsonc
{
  "type": "frame",
  "backgroundVariable": "f330b2b81049533f6a1627c057c12b614b41c9d4",
  "background": {"r": 0.078, "g": 0.078, "b": 0.078}
}
```

### Instance node — bind icon tint variable
```jsonc
{
  "type": "instance",
  "componentKey": "...",
  "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
  "color": {"r": 1, "g": 1, "b": 1}
}
```

### Root page — bind page bg variable
```jsonc
{
  "backgroundVariable": "f9b47b8b07c15cd8accb2b1eb6fa1cd0303457a0",
  "background": {"r": 0, "g": 0, "b": 0}
}
```
