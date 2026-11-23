# Tailwind and shadcn/ui

Tailwind's default spacing scale is already `1 = 4px`, so the 8pt grid means **using only even-numbered steps**, plus `1` (4px) for the half step and `3` (12px). The discipline is about which steps you refuse to use.

## Spacing utilities

| Token | Value | Utility |
|---|---|---|
| `xs` | 4px | `p-1` `gap-1` `m-1` |
| `sm` | 8px | `p-2` |
| `md` | 12px | `p-3` |
| `lg` | 16px | `p-4` |
| `xl` | 24px | `p-6` |
| `2xl` | 32px | `p-8` |
| `3xl` | 48px | `p-12` |
| `4xl` | 64px | `p-16` |
| `5xl` | 96px | `p-24` |

**Avoid:** `p-5` (20px), `p-7` (28px), `p-9` (36px), `p-10` (40px), `p-11` (44px), `p-14` (56px), and every `.5` step (`gap-1.5`, `px-2.5`). They are valid Tailwind but sit off the rhythm. `h-9` and `h-10` are fine — as control heights, not as spacing.

**Never use arbitrary spacing** — `p-[13px]`, `mt-[22px]`, `gap-[7px]`. An arbitrary value is a signal that the layout is being patched rather than composed. The exceptions are genuine one-offs: a negative offset to align an icon optically, or a fixed height matching an external asset. Comment those.

## Typography

Define the scale explicitly as `[size, lineHeight]` tuples so line-height can never be forgotten:

```js
// tailwind.config.js
fontSize: {
  caption: ['0.75rem',  { lineHeight: '1rem' }],      // 12 / 16
  sm:      ['0.875rem', { lineHeight: '1.25rem' }],   // 14 / 20
  base:    ['1rem',     { lineHeight: '1.5rem' }],    // 16 / 24
  lg:      ['1.125rem', { lineHeight: '1.75rem' }],   // 18 / 28
  xl:      ['1.25rem',  { lineHeight: '1.75rem', fontWeight: '600' }],        // 20 / 28
  '2xl':   ['1.5rem',   { lineHeight: '2rem', fontWeight: '600' }],           // 24 / 32
  '3xl':   ['2rem',     { lineHeight: '2.5rem', letterSpacing: '-0.01em' }],  // 32 / 40
  '4xl':   ['2.5rem',   { lineHeight: '3rem',  letterSpacing: '-0.02em' }],   // 40 / 48
  display: ['3.5rem',   { lineHeight: '4rem',  letterSpacing: '-0.02em' }],   // 56 / 64
}
```

With this in place, `text-2xl` carries its own line-height and tracking — never pair it with a separate `leading-*` unless deliberately overriding.

### Font families and weights

Load fonts with `next/font` in Next.js — it self-hosts, subsets, sets `font-display: swap`, and avoids layout shift. Expose each as a CSS variable and build the stacks in the config:

```tsx
// app/layout.tsx
import { Inter, Noto_Sans_TC, JetBrains_Mono } from "next/font/google";

const sans = Inter({ subsets: ["latin"], weight: ["400", "500", "600", "700"], variable: "--font-sans" });
const cjk  = Noto_Sans_TC({ weight: ["400", "500", "700"], variable: "--font-cjk", preload: false });
const mono = JetBrains_Mono({ subsets: ["latin"], weight: ["400"], variable: "--font-mono" });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="zh-Hant" className={`${sans.variable} ${cjk.variable} ${mono.variable}`}>
      <body className="font-sans">{children}</body>
    </html>
  );
}
```

```js
// tailwind.config.js — theme.fontFamily (v3) / @theme --font-* (v4)
fontFamily: {
  sans: ["var(--font-sans)", "system-ui", "-apple-system", "Segoe UI", "Roboto",
         "var(--font-cjk)", "PingFang TC", "Microsoft JhengHei", "sans-serif"],
  mono: ["var(--font-mono)", "ui-monospace", "SFMono-Regular", "Menlo", "Consolas", "monospace"],
},
fontWeight: {             // override, not extend: removes thin/light/extrabold/black
  normal: "400", medium: "500", semibold: "600", bold: "700",
},
```

- **Request only the weights you use.** Each `weight` in `next/font` is a separate download; for CJK it is a large one. Loading Noto Sans TC with 500 here is what keeps `font-medium` from collapsing to 400 on Windows (SKILL.md §2).
- **Overriding `fontWeight`** makes `font-light` and `font-black` produce no CSS, so the three-weight rule enforces itself.
- **Add to `globals.css`:** `html { font-synthesis: none; }` — there is no Tailwind utility for it.
- **`tabular-nums`** is a built-in utility; put it on table cells, stat cards, prices, and timers.

### CJK line-height

The `fontSize` tuples above use Latin line-heights. For CJK-heavy text, override with on-grid `leading-*` rather than creating new sizes:

| Size | Latin | CJK body |
|---|---|---|
| `text-sm` (14px) | 20px (built in) | `leading-6` (24px) |
| `text-base` (16px) | 24px (built in) | `leading-7` (28px) |
| `text-lg` (18px) | 28px (built in) | `leading-8` (32px) |

For a whole Chinese-language app, `:lang(zh) body { line-height: 1.75; }` in `globals.css` sets the default — but every element with a `text-*` class gets the tuple's own line-height, which wins over the inherited one. So on CJK pages, pair size classes with the `leading-*` values above (a `prose`/article wrapper is a good place to do it once). Never use `italic` on CJK text, and drop the negative `letterSpacing` from the heading tuples on CJK pages (`tracking-normal`).

## Motion utilities

Tailwind's built-in easing utilities already match the curves in SKILL.md §6, so no custom values are needed:

| Purpose | Utilities |
|---|---|
| Press feedback | `duration-100 ease-out` |
| Hover, focus | `duration-150 ease-out` |
| Shadow / elevation | `duration-200 ease-out` |
| Entering (dropdown, dialog) | `duration-300 ease-out` — `ease-out` is `cubic-bezier(0, 0, 0.2, 1)` |
| Exiting | `duration-200 ease-in` — `ease-in` is `cubic-bezier(0.4, 0, 1, 1)` |
| Moving on screen | `duration-300 ease-in-out` — `cubic-bezier(0.4, 0, 0.2, 1)` |
| Spinners | `animate-spin` (linear) |

- **Use `transition` or an explicit property list, never `transition-all`.** Tailwind's plain `transition` class covers colors, opacity, box-shadow, and transform — exactly the safe set. For tighter control: `transition-[color,background-color,box-shadow,transform]`.
- **Reduced motion:** use the `motion-reduce:` variant — `motion-reduce:transition-none`, `motion-reduce:active:scale-100`, `motion-reduce:animate-none`. For opt-in motion, `motion-safe:animate-in`.
- **Hover on touch devices:** Tailwind v4's `hover:` only applies on devices that support hover. In v3, enable `future: { hoverOnlyWhenSupported: true }` so hover styles don't stick after a tap.
- **Press scale:** Tailwind v4 accepts `active:scale-98` directly. In v3 the scale utilities stop at 95, so add `scale: { 98: '0.98' }` to `theme.extend` rather than writing `active:scale-[0.98]` everywhere.
- **Enter/exit animations** (Radix/shadcn popovers, dialogs): `tailwindcss-animate` or `tw-animate-css` provide `animate-in fade-in-0 zoom-in-95` / `animate-out fade-out-0 zoom-out-95`. Set `data-[state=open]:duration-300` and `data-[state=closed]:duration-200` so exits are shorter than entrances.

## Making violations impossible

Overriding `spacing` (rather than extending it) removes the off-grid steps from the framework entirely, so a stray `p-5` produces no CSS and gets caught immediately:

```js
// tailwind.config.js — theme, not theme.extend
theme: {
  spacing: {
    0: '0px', px: '1px',
    1: '4px',   2: '8px',   3: '12px',  4: '16px',
    6: '24px',  8: '32px',  9: '36px',  10: '40px',
    12: '48px', 16: '64px', 24: '96px', 32: '128px',
  },
  borderRadius: {
    none: '0px', sm: '4px', DEFAULT: '8px',
    md: '8px', lg: '12px', xl: '16px', full: '9999px',
  },
  extend: {
    scale: { 98: '0.98' },
  },
}
```

`9` and `10` are kept only because `h-9`/`h-10` are standard control heights; treat `p-9`/`p-10`/`gap-10` as violations in review. This override also affects `w-*`, `h-*`, `min-w-*`, and `translate-*`, which share the spacing scale — use fractions (`w-1/2`), `max-w-*`, or commented arbitrary values for widths outside it.

## Tailwind v4

In v4 the scale lives in CSS:

```css
@theme {
  --spacing-1: 4px;
  --spacing-2: 8px;
  --spacing-3: 12px;
  --spacing-4: 16px;
  --spacing-6: 24px;
  --spacing-8: 32px;
  --spacing-12: 48px;
  --spacing-16: 64px;
  --spacing-24: 96px;

  --text-base: 1rem;
  --text-base--line-height: 1.5rem;
  --text-2xl: 1.5rem;
  --text-2xl--line-height: 2rem;
}
```

v4 generates spacing utilities dynamically from a `--spacing` base unit (every integer step works), so the config cannot block `p-5` the way v3's override does. Rely on the audit command below or a lint rule instead.

## shadcn/ui

shadcn components are copied into the repo, so they are yours to normalize — once, in the component file, not per call site.

What current shadcn (new-york v4) ships, and where it departs from this skill:

| Component | shadcn default | Issue |
|---|---|---|
| Button `default` | `h-9 px-4`, `gap-2` | Fine (36px dense) |
| Button `sm` | `h-8 px-3 gap-1.5`, `has-[>svg]:px-2.5` | `gap-1.5` (6px) and `px-2.5` (10px) are off-grid |
| Button `lg` | `h-10 px-6` | Only 40px — not a touch-first CTA size |
| Button `xs` | `h-6` | 24px — below any control minimum except WCAG AA's floor; avoid for primary actions |
| Button base | `transition-all` | Animates everything; replace with an explicit list |
| Button base | `focus-visible:ring-[3px]` | Arbitrary value; `ring-2 ring-offset-2` is on-scale |
| Input | `h-9 px-3` | Matches the default button — keep them matched if you change either |

A normalized `buttonVariants` that applies §5 and §6 of SKILL.md:

```tsx
const buttonVariants = cva(
  [
    "inline-flex shrink-0 items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium",
    // motion: explicit properties, short, ease-out; press feedback; reduced-motion safe
    "transition-[color,background-color,box-shadow,transform] duration-150 ease-out",
    "active:scale-98 motion-reduce:transition-none motion-reduce:active:scale-100",
    // focus: on-scale ring, keyboard only
    "outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
    // disabled: no interaction, no elevation
    "disabled:pointer-events-none disabled:opacity-50 disabled:shadow-none",
    // loading
    "aria-busy:pointer-events-none",
    "[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
  ],
  {
    variants: {
      variant: {
        default:
          "bg-primary text-primary-foreground shadow-sm hover:bg-primary/90 hover:shadow-md active:shadow-sm",
        destructive:
          "bg-destructive text-white shadow-sm hover:bg-destructive/90 hover:shadow-md active:shadow-sm",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        outline: "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        sm: "h-8 min-w-16 gap-1 px-3",
        default: "h-10 min-w-16 px-4 has-[>svg:first-child]:pl-3",
        dense: "h-9 min-w-16 px-4 has-[>svg:first-child]:pl-3",
        lg: "h-12 min-w-24 px-6 text-base [&_svg:not([class*='size-'])]:size-5",
        icon: "size-10",
        "icon-sm": "size-8",
        "icon-lg": "size-12",
      },
    },
    defaultVariants: { variant: "default", size: "default" },
  }
)
```

Notes:

- **If you move the default to `h-10`, change `Input`, `SelectTrigger`, and similar controls to `h-10` too.** Otherwise pick `dense` (`h-9`) as the project default and leave inputs alone. The point is that they match.
- **`has-[>svg:first-child]:pl-3`** trims only the leading-icon side (12px / 16px), per SKILL.md §5.
- **Loading state** — keep width stable and announce it:

  ```tsx
  <Button aria-busy={loading} disabled={loading}>
    {loading ? <Loader2 className="animate-spin" /> : <Save />}
    Save changes
  </Button>
  ```

  Swapping the icon (not the label) keeps the width fixed.
- **`--radius`** in `globals.css` drives every component's rounding. Set it to 8px (`0.5rem`) and let `rounded-md` / `rounded-lg` derive from it.
- **Card internals** ship with `p-6` (24px) throughout. Keep header, content, and footer consistent — mixed padding inside one card reads as a rendering bug.
- **Keep semantic color tokens.** `bg-background` / `text-muted-foreground` / `border-border` give dark mode for free. Replacing them with `bg-white` or `text-gray-500` breaks theming, and `text-gray-400` on white fails contrast at about 2.8:1.

## Quick audit command

To find off-grid utilities, arbitrary values, and `transition-all` in an existing codebase:

```bash
grep -rnE '\b(p|px|py|pt|pb|pl|pr|m|mx|my|mt|mb|ml|mr|gap|gap-x|gap-y|space-[xy])-(5|7|9|10|11|13|14|15|[0-9]+\.5)\b' src/
grep -rnE '(p|m|gap|text|leading|duration|ring)-\[' src/
grep -rn 'transition-all' src/
```
