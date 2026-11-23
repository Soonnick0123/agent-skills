# Plain CSS, React Native, and Flutter

The scales in SKILL.md are unit-agnostic. What changes per platform is how they are declared and which density unit carries them.

## Plain CSS

Declare tokens as custom properties and reference them everywhere. A raw `px` value appearing in a rule is the thing to catch in review.

```css
:root {
  /* Spacing — 8pt grid */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-lg: 16px;
  --space-xl: 24px;
  --space-2xl: 32px;
  --space-3xl: 48px;
  --space-4xl: 64px;
  --space-5xl: 96px;

  /* Type — size and line-height always paired */
  --text-caption: 0.75rem;   --lh-caption: 1rem;
  --text-sm: 0.875rem;       --lh-sm: 1.25rem;
  --text-base: 1rem;         --lh-base: 1.5rem;
  --text-lg: 1.125rem;       --lh-lg: 1.75rem;
  --text-h3: 1.5rem;         --lh-h3: 2rem;
  --text-h2: 2rem;           --lh-h2: 2.5rem;
  --text-h1: 2.5rem;         --lh-h1: 3rem;

  /* Radius */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;

  /* Elevation — three levels, no more */
  --shadow-sm: 0 1px 2px rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 12px rgb(0 0 0 / 0.08);
  --shadow-lg: 0 12px 32px rgb(0 0 0 / 0.12);

  /* Control heights */
  --control-sm: 32px;
  --control-md: 40px;   /* 36px in dense UI */
  --control-lg: 48px;

  /* Motion — durations */
  --duration-instant: 100ms;
  --duration-fast: 150ms;
  --duration-normal: 200ms;
  --duration-moderate: 300ms;
  --duration-slow: 400ms;

  /* Motion — easing */
  --ease-enter: cubic-bezier(0, 0, 0.2, 1);     /* decelerate */
  --ease-exit: cubic-bezier(0.4, 0, 1, 1);      /* accelerate */
  --ease-move: cubic-bezier(0.4, 0, 0.2, 1);    /* standard */
}
```

A button built from these tokens:

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-sm);
  height: var(--control-md);          /* explicit height, not vertical padding */
  min-width: 64px;
  padding-inline: var(--space-lg);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  line-height: var(--lh-sm);
  font-weight: 500;
  white-space: nowrap;
  transition:
    background-color var(--duration-fast) var(--ease-enter),
    box-shadow var(--duration-normal) var(--ease-enter),
    transform var(--duration-instant) var(--ease-enter);
}
.btn-primary { box-shadow: var(--shadow-sm); }
@media (hover: hover) {
  .btn-primary:hover { box-shadow: var(--shadow-md); }
}
.btn:active { transform: scale(0.98); }
.btn-primary:active { box-shadow: var(--shadow-sm); }   /* back to resting, not zero */
.btn:focus-visible { outline: 2px solid var(--color-focus); outline-offset: 2px; }
.btn:disabled { opacity: 0.5; box-shadow: none; cursor: not-allowed; }
.btn[aria-busy="true"] { pointer-events: none; }

@media (prefers-reduced-motion: reduce) {
  .btn { transition: background-color var(--duration-fast) linear; }
  .btn:active { transform: none; }
}
```

Wrap hover styles in `@media (hover: hover)` so they don't stick after a tap on touch screens.

Use `rem` for font sizes so browser zoom and user font-size preferences work — a hard accessibility requirement. Use `px` for spacing, borders, and radii, which should not scale with text.

Two techniques that keep the grid intact under real content:

- **`clamp()` for fluid type** still needs grid-aligned endpoints: `clamp(2rem, 5vw, 2.5rem)` moves between two scale steps rather than landing anywhere.
- **Prefer `gap` over margins** in flex and grid layouts. Margins collapse and accumulate unpredictably; `gap` cannot, so the rhythm survives content changes.

Font families and weights in plain CSS:

```css
:root {
  --font-sans: "Inter", system-ui, -apple-system, "Segoe UI", Roboto,
               "PingFang TC", "Noto Sans TC", "Microsoft JhengHei", sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace;
  --weight-regular: 400;
  --weight-medium: 500;
  --weight-semibold: 600;
  --weight-bold: 700;
}
html { font-family: var(--font-sans); font-synthesis: none; }
:lang(zh), :lang(ja) { line-height: 1.75; letter-spacing: 0; font-style: normal; }
.num, td.numeric { font-variant-numeric: tabular-nums; }

@font-face {
  font-family: "Inter";
  src: url("/fonts/inter-var.woff2") format("woff2");
  font-weight: 400 700;          /* variable font: one file, whole range */
  font-display: swap;
}
```

Declare one `@font-face` per static weight (or one with a range for a variable font) and never reference a weight you haven't declared.

## React Native

React Native's unit is the density-independent pixel, which maps to the same numbers — the 8pt scale transfers directly. Export a token module and forbid inline numbers.

```ts
// theme.ts
export const space = {
  xs: 4, sm: 8, md: 12, lg: 16,
  xl: 24, xxl: 32, xxxl: 48, huge: 64,
} as const;

export const type = {
  caption:  { fontSize: 12, lineHeight: 16 },
  bodySm:   { fontSize: 14, lineHeight: 20 },
  body:     { fontSize: 16, lineHeight: 24 },
  h3:       { fontSize: 20, lineHeight: 28, fontWeight: '600' },
  h2:       { fontSize: 24, lineHeight: 32, fontWeight: '600' },
  h1:       { fontSize: 32, lineHeight: 40, fontWeight: '700' },
} as const;

export const radius = { sm: 4, md: 8, lg: 12, xl: 16, full: 9999 } as const;

export const control = { sm: 32, md: 40, lg: 48 } as const;
export const iconSize = { button: 20, standalone: 24 } as const;   // native runs one step above web

export const duration = {
  instant: 100, fast: 150, normal: 200, moderate: 300, slow: 400,
} as const;

// Reanimated: import { Easing } from 'react-native-reanimated'
export const easing = {
  enter: Easing.bezier(0, 0, 0.2, 1),
  exit:  Easing.bezier(0.4, 0, 1, 1),
  move:  Easing.bezier(0.4, 0, 0.2, 1),
};
```

Buttons: use `Pressable` with a fixed `height` from `control`, `paddingHorizontal` from `space`, and `hitSlop` to reach 44pt (iOS) / 48dp (Android) when the visual is 32 or 40. Drive pressed styling from Pressable's `pressed` state — `transform: [{ scale: 0.98 }]` and an overlay — rather than `TouchableOpacity`'s blanket opacity fade, which dims text below contrast minimums mid-press. On Android, `android_ripple` is the platform-native press feedback and can replace the scale.

Reduced motion: check `AccessibilityInfo.isReduceMotionEnabled()` (or Reanimated's `useReducedMotion()`) and skip transform and layout animations when it returns true; keep short opacity fades.

Fonts in React Native:

- **On Android, `fontWeight` does not select weights of a custom font.** Each weight is a separate file and usually a separate `fontFamily` name (`Inter-Regular`, `Inter-Medium`, `Inter-SemiBold`). Map weights to family names in the theme — `type.body = { fontFamily: 'Inter-Regular', ... }` — rather than setting `fontWeight: '600'` and hoping. iOS is more forgiving, which is why this bug usually ships unnoticed.
- **Load fonts before first render** (`expo-font`'s `useFonts`, or linked assets) and keep the splash screen up until they resolve; otherwise the first frame renders in the system font.
- **CJK:** the system fonts (PingFang on iOS, Noto Sans CJK on Android) cover Chinese and Japanese with proper weights, so bundling a CJK font is rarely worth the size. Set a larger `lineHeight` (≈1.7×) on CJK text styles.
- **Numbers:** `fontVariant: ['tabular-nums']` on counters, prices, and table columns.

Platform notes that affect the grid:

- **`lineHeight` is required, not optional.** React Native's default line-height varies by platform and font, so vertical rhythm drifts between iOS and Android unless set explicitly.
- **Minimum touch target is 44pt on iOS and 48dp on Android.** Use `hitSlop` to extend the tappable area without changing the visual size.
- **Respect safe areas** with `react-native-safe-area-context` rather than hardcoding top padding. Notch and home-indicator insets are not grid values and should not be treated as spacing.
- **`allowFontScaling` defaults to true** — text grows with the user's system font setting. Lay out with flexible heights rather than fixed ones, or large-text users get clipped content.

## Flutter

Flutter uses logical pixels, again the same numbers. Define constants rather than scattering `EdgeInsets.all(13)`.

```dart
class Spacing {
  static const double xs = 4;
  static const double sm = 8;
  static const double md = 12;
  static const double lg = 16;
  static const double xl = 24;
  static const double xxl = 32;
  static const double xxxl = 48;
}
```

Flutter-specific notes:

- **Use `TextTheme` in `ThemeData`** instead of per-widget `TextStyle`. Set `fontSize` and `height` together, where `height` is a *multiplier*, not a pixel value: a 16px body with a 24px line-height is `height: 1.5`. Compute it as `targetLineHeight / fontSize` and confirm the product lands on a multiple of 4.
- **Material defaults are not the 8pt grid.** `ListTile` and several Material components carry their own padding — override `contentPadding` rather than wrapping in a `Padding` that fights it.
- **`SizedBox` is the idiomatic gap** between children in a `Column` or `Row`; use the spacing constants for its height and width.
- **Minimum touch target is 48dp.** `materialTapTargetSize` enforces this; leave it on `padded`.
- **`MediaQuery.textScalerOf(context)`** reflects the user's text-size setting. Avoid fixed-height containers around scalable text.
- **Buttons:** Material 3's `FilledButton` already defaults to 40dp height, 16dp horizontal padding, 20dp icons, and fully rounded corners — the `md` row of SKILL.md §5. Override through `FilledButtonThemeData(style: ButtonStyle(minimumSize: …, padding: …))` in the theme, not per widget. For the 48px `lg` CTA, set `minimumSize: Size(64, 48)`.
- **Motion:** Flutter's `Easing` class (Material 3) provides `Easing.emphasizedDecelerate` for entering, `Easing.emphasizedAccelerate` for exiting, and `Easing.standard` for on-screen movement. Durations live in `Durations.short2` (100ms), `short3` (150ms), `short4` (200ms), `medium2` (300ms), `medium4` (400ms). Use these rather than `Curves.bounceOut` or ad-hoc `Duration(milliseconds: 350)`.
- **Reduced motion:** `MediaQuery.disableAnimationsOf(context)` returns true when the OS asks for reduced motion. Gate transform and slide animations on it — pass `Duration.zero` or skip the transition — while keeping short fades.
- **Fonts:** declare every weight in `pubspec.yaml` with an explicit `weight:` (400/500/600/700) against its own file. If a requested `FontWeight` has no matching file, Flutter synthesizes a fake bold. Set `fontFamily` once in `ThemeData`, and use `fontFamilyFallback` for CJK (`['PingFang TC', 'Noto Sans TC', 'Microsoft JhengHei']`).
- **CJK locale:** set `locale`/`supportedLocales` (or `TextStyle(locale: …)` on mixed-language text) so shared Han characters render with the right regional glyphs; bump `height` to ~1.7 on CJK text styles.
- **Numbers:** `fontFeatures: [FontFeature.tabularFigures()]` on counters, prices, and table columns.

## Density variants

Data-dense UI (tables, admin panels, IDE-like tools) legitimately needs tighter spacing than a marketing page. Handle that by shifting the whole scale down one step — 24px becomes 16px, 16 becomes 12, 12 becomes 8 — rather than by inventing intermediate values. The rhythm survives; only the tempo changes.
