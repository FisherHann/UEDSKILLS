# Text Styles (Typography2025)

> Font library text style keys — use `textStyle` field in JSON to bind.
> Scanned 2026-03-26. All 20 styles confirmed via `importStyleByKeyAsync`.
> Font: **Inter** (Regular / Semi Bold). All styles bind via hardcoded keys in `code.js`.

---

## Body & Label Styles

| textStyle | key | id | font | fontSize | fontWeight | lineHeight | Use for |
|-----------|-----|----|------|----------|------------|------------|---------|
| `Semibold/24` | `63a28af908557280e4a282f03952ab43729013e5` | `S:...,2609:14` | Inter Semi Bold | 24 | 600 | 32 | Page titles |
| `Regular/24` | `5acafeed78c82d6266ec4251934e63b3d45edd16` | `S:...,2609:6` | Inter Regular | 24 | 400 | 32 | Page titles (regular) |
| `Semibold/20` | `35fb29567075d8502fc1f847e70a5db565d94d47` | `S:...,2609:13` | Inter Semi Bold | 20 | 600 | 28 | Section titles |
| `Regular/20` | `b15d2b35622f3d5fdf33bbc260ebfda89968eebb` | `S:...,2609:5` | Inter Regular | 20 | 400 | 28 | Section titles (regular) |
| `Semibold/18` | `c263f6c79f9e992bc6d3b1e0762fea8253720a0e` | `S:...,2609:12` | Inter Semi Bold | 18 | 600 | 26 | Card titles, panel titles |
| `Regular/18` | `4b09ca90512fc5f4cef26f35f843551a2c390650` | `S:...,2609:4` | Inter Regular | 18 | 400 | 26 | Large body |
| `Semibold/16` | `fb6b05c3ebc979b4616801a01aea9b2991a281c7` | `S:...,2609:11` | Inter Semi Bold | 16 | 600 | 24 | Key metrics, last price |
| `Regular/16` | `eef16a67cb4eba4134ab99e4bdd56bf24c937d5c` | `S:...,2609:3` | Inter Regular | 16 | 400 | 24 | Default body font |
| `Semibold/14` | `95c43064b7107a35a1c85e28bca7fd870c03dda7` | `S:...,2609:10` | Inter Semi Bold | 14 | 600 | 22 | Active tab, button text |
| `Regular/14` | `9e2e5ce326940a39f1a397744f6b126b62c01512` | `S:...,2609:2` | Inter Regular | 14 | 400 | 22 | Body text, descriptions |
| `Semibold/12` | `ad093050bf6f479e6c382e0317c64bac1bb428af` | `S:...,2609:9` | Inter Semi Bold | 12 | 600 | 18 | Small bold label, tag |
| `Regular/12` | `8d1cf8fd6e5f75224901553b25d4f1d075e654e7` | `S:...,2609:1` | Inter Regular | 12 | 400 | 18 | Helper text, footer |
| `Semibold/10` | `7f31e3a76bcf8dd27330216dbc054c6602dcbd15` | `S:...,2609:8` | Inter Semi Bold | 10 | 600 | 14 | Micro bold |
| `Regular/10` | `b271c0aacc59f19ad17fe6ec5252470c211ea2b4` | `S:...,2609:0` | Inter Regular | 10 | 400 | 14 | Micro text |
| `Semibold/8` | `445a05f4f02efe11441deab95ff53cbb940768a4` | `S:...,2609:7` | Inter Semi Bold | 8 | 600 | 10 | Badge text only |

## Headline Styles

| textStyle | key | id | font | Use for |
|-----------|-----|----|------|---------|
| `Headline/H0` | `3d84b173c10dc34387d472664b6ce9b88202968e` | `S:...,2609:19` | Inter Semi Bold | Hero display (largest) |
| `Headline/H1` | `9cd324675bd41c82c327e608fc20c1ab168ac796` | `S:...,2609:18` | Inter Semi Bold | Page hero title |
| `Headline/H2` | `7b01ed1acc13404cc7f99c5b28fce787b415192e` | `S:...,2609:17` | Inter Semi Bold | Major section title |
| `Headline/H3` | `4e171655f221ea9ce5a1348e2d2666930040567d` | `S:...,2609:16` | Inter Semi Bold | Sub-section title |
| `Headline/H4` | `b6b842624757ce115b2e479d11d14e4c9c0ea25f` | `S:...,2609:15` | Inter Semi Bold | Card/block title |

## H5 Only Variants (Web/H5, not for App)

| textStyle | key |
|-----------|-----|
| `Headline/H0 (H5 Only)` | `93383874635923f9ce7d24e70daee43aa4e86273` |
| `Headline/H1 (H5 Only)` | `f14a9ca820d4d8f3d5890d9b6de31f3a2449aa8b` |
| `Headline/H2 (H5 Only)` | `7c9e1a612f96007d89a54243ba9b58ffb430333b` |
| `Headline/H3 (H5 Only)` | `9b8ca5c0fc564cbb31d8b8b6423c48df22935da3` |
| `Headline/H4 (H5 Only)` | `b1801ca7d6a4873f358ff9f71005c39e6a22d26c` |

---

## Binding Mechanism

Plugin uses `figma.importStyleByKeyAsync(key)` to import the style, then applies via `text.textStyleId = style.id`.

Keys are **hardcoded** in `code.js` (`_textStyleKeys` map) — no runtime scanning needed.

### JSON Usage

Always include **both** `textStyle` and manual fallback. Plugin binds the library style first; falls back to manual values if binding fails.

```jsonc
{
  "type": "text",
  "content": "Section Title",
  "textStyle": "Semibold/20",
  "fontSize": 20, "fontWeight": 600, "lineHeight": 28,
  "colorVariable": "06604334971ef5a010e564cf2fddd532e6b52e51",
  "color": {"r": 1, "g": 1, "b": 1}
}
```

The `textStyle` value must exactly match a name in the tables above (e.g. `"Semibold/14"`, `"Headline/H2"`).
