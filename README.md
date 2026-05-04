# 0xrodsan.github.io

Personal bilingual (PT-BR / EN) website of **0xRodsan**, hosted on GitHub Pages at <https://0xrodsan.github.io>.

Built with vanilla HTML5, modern CSS (custom properties), and plain JavaScript — no frameworks, no build step.

## Structure

```
.
├── index.html        # Brazilian Portuguese version (root)
├── en/
│   └── index.html    # English version
├── style.css         # Shared styles (light + dark themes)
├── script.js         # Theme toggle, mobile menu, reveal animations
└── README.md
```

## Features

- **Bilingual routing**: `/` serves Portuguese, `/en/` serves English. Proper `hreflang` and canonical tags on each page.
- **Dark / light theme**: Defaults to the user's `prefers-color-scheme`, can be overridden via the toggle, and the choice is persisted in `localStorage`. A small inline script in `<head>` applies the theme before paint to avoid a flash of the wrong theme.
- **Mobile-first responsive layout** with a hamburger menu on small screens and a horizontal nav on tablet and up.
- **Fade-in animations** on sections via `IntersectionObserver`, with `prefers-reduced-motion` respected.
- **Minimalist design**: serif headings (Fraunces), sans-serif body (Inter), generous whitespace, subtle borders.
- **Accessible**: semantic landmarks, ARIA labels, keyboard-friendly controls.

## Local development

No build step. Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

Push to the `main` branch of the `0xrodsan.github.io` repository. GitHub Pages will serve the static files directly from the root.

## Editing content

- **Portuguese copy** lives in `index.html`.
- **English copy** lives in `en/index.html`.
- Both files share `style.css` and `script.js` via relative links.
- To add new nav items (e.g. Articles, Reports), append `<li class="nav-item">` entries to `.nav-list` and `.mobile-nav-list` in both HTML files.

## License

Personal site — content © Rodrigo Santos. Code is free to reuse for your own personal site.
