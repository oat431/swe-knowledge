---
tags:
- hci
- software-design
- software-engineering
- ui
- ux
---

# 09 Dark Mode

Dark mode isn't just "white background becomes black." It's a complete rethinking of contrast, depth, and visual hierarchy. Done wrong, it's harder to read than light mode.

---

## Why Dark Mode Matters

| Reason | Detail |
|--------|--------|
| **User demand** | Many users prefer it — especially developers and night-time users |
| **Battery saving** | On OLED screens, black pixels use zero power |
| **Accessibility** | Users with light sensitivity or visual impairments rely on it |
| **Expectation** | OS-level dark mode support is now standard — apps that don't support it feel outdated |

---

## The Core Principle: Elevation

In light mode, shadows create depth. In dark mode, **light creates depth** — higher surfaces are lighter.

```
Light Mode:                    Dark Mode:
┌──────────┐ ← shadow          ┌──────────┐ ← lighter (elevated)
│  Card    │                   │  Card    │
└──────────┘                   └──────────┘
  background = white             background = dark gray
                                 
                                ┌──────────┐ ← even lighter (highest)
                                │  Modal   │
                                └──────────┘
                                  page bg = near-black
```

---

## Color Rules for Dark Mode

| ❌ Don't | ✅ Do |
|----------|------|
| Use pure black `#000000` as background | Use dark gray `#121212` or `#1a1a1a` — pure black causes eye strain |
| Use pure white `#FFFFFF` for text | Use off-white `#E0E0E0` or `#F5F5F5` — pure white glares on dark backgrounds |
| Keep saturated colors at full brightness | **Desaturate** colors by 20–30% — saturated colors vibrate on dark backgrounds |
| Use the same shadows as light mode | Shadows don't work on dark — use **lighter elevation** instead |

### Desaturation Example

| Color | Light Mode | Dark Mode |
|-------|-----------|-----------|
| Red | `#EF4444` | `#F87171` (lighter, less saturated) |
| Blue | `#3B82F6` | `#60A5FA` (lighter, less saturated) |
| Green | `#22C55E` | `#4ADE80` (lighter, less saturated) |

---

## Contrast in Dark Mode

Dark mode needs **lower contrast** than light mode for comfort:

| Element | Light Mode | Dark Mode |
|---------|-----------|-----------|
| Body text on bg | `#111` on `#FFF` (17:1) | `#E0E0E0` on `#121212` (12:1) — still passes AAA |
| Secondary text | `#666` on `#FFF` (5.5:1) | `#A0A0A0` on `#121212` (6:1) |
| Borders | `#E0E0E0` | `#333333` — subtle, not invisible |

---

## Implementation

```css
/* System preference detection */
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #121212;
    --surface: #1E1E1E;
    --text: #E0E0E0;
    --text-secondary: #A0A0A0;
    --border: #333333;
  }
}
```

> Always provide a manual toggle too — don't force users to change their OS setting.

---

## Sources

- Material Design — Dark Theme: https://m3.material.io/styles/color/dark-theme
- Apple HIG — Dark Mode: https://developer.apple.com/design/human-interface-guidelines/dark-mode
