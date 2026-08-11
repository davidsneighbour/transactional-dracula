# DNB universe site skeleton

Minimal Astro + Tailwind CSS skeleton for small informational and utility pages in the DNB universe.

## Design goals

- one-page-first architecture
- Astro with no client framework
- Tailwind CSS 4 via the Vite plugin
- Tailwind Typography for readable text-heavy pages
- Dracula Classic dark theme
- Alucard Classic light theme
- theme colours mapped from the official Dracula specification
- Dracula red (`#ff5555`) in dark mode and Alucard red (`#cb3a2a`) in light mode
- DNB corporate font stack
- compact 15px base type size
- horizontally centred content
- vertically centred content when the page is shorter than the viewport
- responsive by default
- system colour preference on first visit
- persistent manual light/dark override via `localStorage`
- no remote font or runtime dependency

## Font stack

```css
font-family:
  "Verb",
  "Lato",
  "Noto Sans",
  Arial,
  Helvetica,
  "Liberation Sans",
  sans-serif;
```

The template does not download fonts. If Verb is installed or provided by the host project it is used; otherwise the normal fallback chain applies.

## Install as a new project

Copy the skeleton, then run:

```bash
npm install
npm run dev
```

Production check and build:

```bash
npm run build
```

## Add to an existing Astro project

Install the required packages:

```bash
npm install tailwindcss @tailwindcss/vite @tailwindcss/typography
```

Add the Tailwind Vite plugin to `astro.config.mjs`:

```js
import tailwindcss from '@tailwindcss/vite';
import { defineConfig } from 'astro/config';

export default defineConfig({
  vite: {
    plugins: [tailwindcss()],
  },
});
```

Copy:

```text
src/styles/global.css
```

Import it from the page that needs the DNB theme:

```astro
---
import '../styles/global.css';
---
```

For a one-page project, copy `src/pages/index.astro` and replace the page content.

## Add to an existing non-Astro project

The important reusable part is `src/styles/global.css`. Tailwind must be processed by the host project's Tailwind 4 build pipeline.

The theme uses a `.dark` class on the root `<html>` element. A host project can keep its own theme switcher as long as it toggles that class.

## Change the theme

Corporate design values are concentrated in two places in `src/styles/global.css`. The current light/dark palettes are Alucard Classic and Dracula Classic from the official Dracula specification:

1. `@theme` contains reusable Tailwind design tokens.
2. `:root` and `:root.dark` contain semantic UI colours.

Prefer changing semantic variables instead of replacing colour utilities throughout pages:

```css
:root {
  --page-bg: ...;
  --surface: ...;
  --border: ...;
  --text: ...;
  --text-muted: ...;
  --primary: ...;
  --primary-hover: ...;
  --primary-soft: ...;
  --focus: ...;
}
```

This keeps future corporate-design changes localised.

## Typography

Use `prose prose-sm prose-dnb` for normal informational content:

```html
<div class="prose prose-sm prose-dnb max-w-none">
  ...
</div>
```

`prose-sm` intentionally keeps these utility pages slightly more compact than a normal editorial site.

## Theme behaviour

On first load, the page follows `prefers-color-scheme`. After a visitor toggles the theme, the selected value is stored under:

```text
dnb-theme
```

Accepted values are `light` and `dark`.

## Growing beyond one page

Do not add a layout pre-emptively. Once multiple pages need the same `<head>`, wrapper, footer, or theme switcher, extract them into a layout or components at that point.
