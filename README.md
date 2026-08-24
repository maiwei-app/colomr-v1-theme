# colomr-v1

A personal portfolio theme for [Hugo](https://gohugo.io/) built with **Material Design 3**. Designed with [Google Stitch 2](https://stitch.withgoogle.com) and implemented with [Claude](https://claude.ai).

Dark mode by default, responsive, lightweight, and fully configurable from front matter — no HTML editing needed.

![Screenshot](images/screenshot.png)

## Features

- Dark / Light mode toggle (dark by default)
- Cover images with configurable overlay effects (glass, vignette, shadow, highlight)
- Notion-style block system for interior pages (text, cards, timeline, contact)
- Provider tabs with badge grid (for certifications/courses)
- Material Symbols + Font Awesome icons
- Responsive: desktop nav + mobile bottom nav
- SCSS with MD3 design tokens (easy color/font customization)
- **Multilingual support** (i18n) with language selector and browser detection
- Google Analytics support

## Requirements

- Hugo Extended >= 0.156.0
- Google Fonts loaded via CDN (Plus Jakarta Sans, Inter, Material Symbols)

## Installation

```bash
cd your-hugo-site
git submodule add https://github.com/maiwei-app/colomr-v1-theme.git themes/colomr-v1
```

Set the theme in `hugo.toml`:

```toml
theme = "colomr-v1"
```

## Quick start

Copy the `exampleSite/` contents to your project root and run:

```bash
hugo server
```

## Configuration

### hugo.toml

```toml
baseurl = "https://example.com/"
title = "My Personal Site"
theme = "colomr-v1"
languagecode = "en"

# Disable taxonomies (not used)
[taxonomies]

[params]
  author = "Jane Doe"
  description = "Software Engineer"
  keywords = "portfolio, cloud, engineering"
  avatarurl = "/images/avatar.png"
  favicon_32 = "/images/favicon-32x32.png"
  favicon_16 = "/images/favicon-16x16.png"
  since = 2024

# Social links (displayed in footer)
[[params.social]]
  name = "Github"
  icon = "fa-brands fa-github fa-2x"
  weight = 1
  url = "https://github.com/"

# Navigation links (icon = Material Symbols name)
[[params.nav_links]]
  label = "About"
  url = "/about/"
  nav_icon = "person"
```

### Multilingual (i18n)

The theme has built-in multilingual support. To enable it, configure Hugo's `[languages]` blocks in `hugo.toml`:

```toml
defaultContentLanguage = "es"
defaultContentLanguageInSubdir = true   # all languages under /es/, /en/ prefixes

[languages.es]
  locale = "es"
  label = "Español"
  weight = 1

[languages.es.params]
  description = "Site description in Spanish"

[[languages.es.params.nav_links]]
  label = "Quién"
  url = "/sobre-mi/"
  nav_icon = "person"

[languages.en]
  locale = "en"
  label = "English"
  weight = 2

[languages.en.params]
  description = "Site description in English"

[[languages.en.params.nav_links]]
  label = "Who"
  url = "/about/"
  nav_icon = "person"
```

#### Content files

Use Hugo's "translation by filename" approach. Rename content files with language suffixes:

```
content/
  _index.es.md          # Spanish home
  _index.en.md          # English home
  about/
    index.es.md         # url: "/es/about/"
    index.en.md         # url: "/en/about/"
```

When using custom `url:` in front matter, include the language prefix (e.g., `url: "/es/sobre-mi/"`), because Hugo does not prepend it automatically for explicit URLs.

#### i18n strings

Create `i18n/es.toml` and `i18n/en.toml` in your project root. The theme uses these translation keys:

| Key | Usage | Example (ES) | Example (EN) |
|-----|-------|-------------|-------------|
| `home_label` | Home link in nav | Inicio | Home |
| `explore_btn` | Button on learning cards | Explorar | Explore |
| `page_under_construction` | Empty blocks fallback | Página en construcción. | Page under construction. |
| `toggle_theme` | Theme toggle aria-label | Cambiar modo de color | Toggle color mode |
| `open_menu` | Hamburger aria-label | Abrir menú | Open menu |
| `footer_credit` | Footer credit prefix | Desarrollado con | Built with |
| `footer_using` | Footer credit connector | usando | using |
| `switch_language` | Language selector label | EN | ES |
| `nav_aria` | Bottom nav aria-label | Navegación principal | Main navigation |

#### Language selector

A text-based toggle (e.g., "EN" / "ES") appears automatically next to the theme toggle when pages have translations. It saves the user's choice to `localStorage` under the key `colomr-lang`.

#### Badge descriptions

Badge data files support a `desc_en` field for English descriptions alongside the default `desc` (Spanish). The template selects the right field based on the current language:

```json
{
  "titulo": "Badge Name",
  "desc": "Descripción en español.",
  "desc_en": "Description in English."
}
```

#### Browser language detection

For the root URL redirect (`/` → `/es/` or `/en/`), create a `layouts/alias.html` in your project that detects the browser language via `navigator.languages` and checks `localStorage` for a saved preference. See the [colomr.cc repo](https://github.com/colomr-cc/colomr.cc/blob/main/layouts/alias.html) for an example implementation.

#### SEO

The theme automatically generates `<link rel="alternate" hreflang="...">` tags when translations exist. Hugo also generates per-language sitemaps automatically.

### Static assets

Place these files in `static/images/`:

| File | Purpose | Recommended size |
|------|---------|-----------------|
| `logo.png` | Site logo (header) | 120x120 px |
| `avatar.png` | Author avatar (header) | 200x200 px |
| `favicon-32x32.png` | Browser favicon | 32x32 px |
| `favicon-16x16.png` | Browser favicon | 16x16 px |

## Layouts

### Home page (`index.html`)

The home page reads from `content/_index.md` front matter. Three sections:

```yaml
hero:
  cover: "/images/covers/hero.webp"          # background image (optional)
  cover_opacity: "0.55"                       # overlay opacity 0-1 (default 0.65)
  cover_effect: "glass"                       # glass | vignette | shadow | highlight
  cover_position: "center center"             # CSS background-position
  chips: ["CLOUD", "AI"]
  title: "Build, Ship &"
  titleHighlight: "Scale"                     # gradient text
  subtitle: "Your tagline here"
  subtitleEmphasis: "in italics"
  subtitleEmoji: "🚀"
  ctas:
    - label: "About me"
      url: "/about/"
      style: "primary"                        # primary | ghost

learning:
  title: "Section Title"
  subtitle: "Section subtitle"
  cards:
    - name: "Card Name"
      desc: "Description"
      url: "/link/"
      icon: "my-icon"                         # loads partials/icons/my-icon.html (optional)
      cssModifier: ""                         # extra CSS class (optional)

context:
  heading: "A bold statement"
  body: "Supporting text. Use <span class=\"context__emphasis\">emphasis</span> for highlights."
  icon: "architecture"                        # Material Symbol name
  stat: "10+"
  statLabel: "YEARS OF EXPERIENCE"
```

### Blocks page (`blocks.html`)

For interior pages with a Notion-style block system. Set `layout: "blocks"` in front matter.

```yaml
layout: "blocks"
cover: "/images/covers/about.webp"
cover_ratio: "3 / 1"                          # 3/1 (default) | 16/9 | 4/1
cover_position: "center center"
cover_opacity: "0.5"
icon: "👩‍💻"
pageTitle: "Page Title"
subtitle: "Subtitle"
tags_list:
  - label: "Tag"

blocks:
  - type: "text"
    heading: "Heading"
    body: "Paragraph text."

  - type: "cards"
    heading: "Heading"
    items:
      - icon: "cloud"                         # Material Symbol name
        title: "Card Title"
        body: "Card description"

  - type: "timeline"
    heading: "Experience"
    items:
      - role: "Job Title"
        company: "Company"
        period: "2022 – present"

  - type: "contact"
    heading: "Get in touch"
    body: "Optional intro text."              # displayed above links
    items:
      - label: "LinkedIn"
        url: "https://linkedin.com/"
        icon: "fa-brands fa-linkedin-in"      # Font Awesome 7 class
```

### Providers page (`providers.html`)

For displaying badges/certifications organized by provider tabs. Set `layout: "providers"` in front matter.

```yaml
layout: "providers"

intro:
  heading: "Intro Title"
  body: "Intro paragraph."

providers:
  - id: "provider-a"                          # used for tab switching
    name: "Provider A"                        # tab label
    profile_url: "https://..."                # link button (omit to hide)
    profile_label: "View my profile"
    data: "badges_a"                          # loads data/badges_a.json
```

#### Badge data format

Create a JSON file in `data/` for each provider:

```json
[
  {
    "titulo": "Badge Name",
    "url": "https://link-to-badge",
    "img": "https://badge-image-url.png",
    "fecha": "2025-03-01"
  }
]
```

The theme displays the **6 most recent** badges per provider, sorted by `fecha`.

## Customization

### Colors and fonts

Edit `assets/scss/_tokens.scss`. All colors follow the MD3 color system. Use [Material Theme Builder](https://material-foundation.github.io/material-theme-builder/) to generate a palette and replace the CSS custom properties.

Key variables:

| Variable | Purpose |
|----------|---------|
| `--color-primary` | Brand color, CTAs, active states |
| `--color-background` | Page background |
| `--color-on-surface` | Main text color |
| `--font-display` | Headings font |
| `--font-body` | Body text font |
| `--gradient-primary` | Title highlight gradient |

### Icons

| System | Usage | Browse |
|--------|-------|--------|
| Material Symbols Outlined | UI, nav, block cards | [fonts.google.com/icons](https://fonts.google.com/icons) |
| Font Awesome 7 | Social links, contact | [fontawesome.com/search](https://fontawesome.com/search?ic=free) |

### Adding a new block type

1. Add `{{ else if eq .type "myblock" }}` in `layouts/_default/blocks.html`
2. Add styles in `assets/scss/_page.scss`
3. Use it in front matter: `- type: "myblock"`

### Learning card icons

The home page learning cards can optionally display custom icons via Hugo partials. Create `layouts/partials/icons/my-icon.html` with your SVG or `<img>` and reference it with `icon: "my-icon"` in the front matter.

## Commands

```bash
hugo server                          # local dev with live reload
hugo --cleanDestinationDir           # production build

# Run exampleSite
hugo server --source themes/colomr-v1/exampleSite --themesDir ../..
```

## License

This theme is licensed under the [GNU General Public License v3.0](LICENSE).

You are free to use, modify, and distribute this theme, provided that:
- Any derivative work is also licensed under GPL-3.0
- The original author is credited (see below)
- The license file is included in any distribution

## Author

Created by **Francisco Colomer** ([colomr.cc](https://colomr.cc))

Built with [Google Stitch 2](https://stitch.withgoogle.com) (design) and
[Claude](https://claude.ai) (development).

Third-party resources:

- Icons: [Material Symbols](https://fonts.google.com/icons) + [Font Awesome 7](https://fontawesome.com/)
- Fonts: [Plus Jakarta Sans](https://fonts.google.com/specimen/Plus+Jakarta+Sans) + [Inter](https://fonts.google.com/specimen/Inter)

If you use this theme, a link back to the [original repository](https://github.com/maiwei-app/colomr-v1-theme) or to [colomr.cc](https://colomr.cc) is appreciated.
