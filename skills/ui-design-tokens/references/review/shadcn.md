# shadcn/ui projects

Read `components.json` first: `tailwind.config` and `tailwind.css` give the exact config and global-CSS paths, and `aliases.ui` gives the primitives directory (usually `components/ui`).

In a shadcn project the foundation layer is three things, all reviewed in Step 3 — not in Step 4.

## 1. Global CSS

The `:root` and `.dark` variables, the `@theme inline` mappings, and `@layer base`.

- **`--radius`** should be `0.5rem` (8px). shadcn ships `0.625rem` (10px), and the `--radius-sm/md/lg/xl` values derived from it with `calc()` land off-scale. Check the derived values, not just the base.
- **Contrast in both `:root` and `.dark`** for every pair: `foreground`/`background`, `primary-foreground`/`primary`, `secondary-foreground`/`secondary`, `muted-foreground`/`background`, `muted-foreground`/`muted`, `accent-foreground`/`accent`, `destructive`/`background`, `card-foreground`/`card`, `popover-foreground`/`popover`, and `ring` against `background` (3:1). Convert `oklch()` / `hsl()` to sRGB to compute the ratio.
- **`@layer base`** should set `font-synthesis: none` and a reduced-motion rule, and every font variable used in `@theme inline` must actually be loaded.

## 2. Primitives in `aliases.ui`

`button.tsx`, `input.tsx`, `select.tsx`, `dialog.tsx`, `card.tsx`, … are project code, not a library. Review their `cva` variants and base classes against the shadcn table and the normalized `buttonVariants` in `ui-design-tokens` → `references/tailwind.md` → *shadcn/ui*. Report each departure once, at the primitive, with the corrected `cva` snippet.

## 3. Height alignment across primitives

Button default, Input, SelectTrigger, and similar controls must share a height. If a fix changes one, it must change the others too.

## Consequences for the rest of the review

- **Exclude `aliases.ui`** from the Layer 1 pattern scan, Layer 2, and Step 4C, so primitive issues are not reported again per file.
- **Fix at the primitive, not the call site.** If many call sites override a primitive the same way (`<Button className="h-11 px-5">`), the finding is a missing variant: add it to the `cva` and replace the overrides with it.
- **Flag call sites that bypass semantic tokens** — raw palette colors like `bg-white`, `text-gray-500`, `bg-blue-600` break dark mode and often fail contrast. The fix is the matching semantic token (`bg-background`, `text-muted-foreground`, `bg-primary`).
- **Don't fight the defaults the skill accepts** — Button `h-9 px-4` as the dense default, Card `p-6`, and `outline-none` paired with `focus-visible:ring-*` on the same element are all fine.
