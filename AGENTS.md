# AI Agent Guide for hugo-foreign-trade-site

## Project Overview
- **Type**: Hugo static site generator
- **Purpose**: Multilingual (Chinese & English) foreign trade company website
- **Deployment**: Cloudflare Pages via GitHub Actions
- **Build Command**: `hugo --minify` → outputs to `public/`
- **Hugo Version**: 0.121.0 (extended)

## Key Structure

### Content Organization
- **`content/zh/`** - Chinese language content (default, weight=1)
- **`content/en/`** - English language content (weight=2)
- Each language folder mirrors the same structure: `_index.md` (home), `contact/`, `products/`
- Hugo generates bilingual URLs automatically based on `languages` config in `hugo.toml`

### Layouts & Templates
- **`layouts/_default/`** - Base templates (baseof.html, home.html, list.html, single.html)
- **`layouts/partials/`** - Reusable components (header.html, footer.html)
- All templates support language switching via `{{ .Language }}`

### Static Assets
- **`static/css/style.css`** - Main stylesheet
- **`static/images/`** - Images
- **`static/uploads/`** - Uploaded/user content
- **`static/admin/`** - Netlify CMS admin interface

## Common Development Tasks

### Building & Testing
```bash
# Build minified site
hugo --minify

# Local dev server with live reload (auto-watches content/layouts)
hugo server

# Build draft content (includes pages with draft: true)
hugo --buildDrafts
```

### Adding Content
1. Create markdown files in `content/zh/` OR `content/en/` (or both for bilingual)
2. Use frontmatter: `title`, `description`, `date`, `draft`
3. Hugo auto-generates `/` (Chinese) and `/en/` (English) URLs based on file location
4. Menu items are defined in `hugo.toml` → `[menu.main]` sections

### Modifying Templates
- Edit `layouts/_default/baseof.html` for site-wide changes (header, footer, CSS link)
- Modify partials in `layouts/partials/` for component changes
- Use Hugo variables: `{{ .Title }}`, `{{ .Language }}`, `{{ .Site.Title }}`, `{{ .Content }}`
- All content blocks use `{{ block "main" . }}{{ end }}` pattern in baseof.html

### Deployment
- Pushes to `main` branch trigger [deploy.yml](.github/workflows/deploy.yml)
- Requires `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` secrets in GitHub
- Deploys built `public/` directory to Cloudflare Pages project `hugo-foreign-trade`

## Configuration Details

### Languages
- Default language is Chinese (`defaultContentLanguage = "zh"`)
- URLs: Root `/` serves Chinese, `/en/` serves English
- `defaultContentLanguageInSubdir = false` means Chinese URLs don't have `/zh/` prefix

### Hugo Output
- Generates JSON for home pages (`[outputs] home = ["HTML", "JSON"]`)
- Minified output only (no sourcemaps in production)

## Common Conventions
- Content files use lowercase with hyphens (e.g., `product-1.md`)
- All section pages use `_index.md` (not `index.md`)
- Comments in Chinese are in config; English comments in templates
- No theme submodule visible in repo root (may be in `.gitmodules`)

## Agent Notes
- Hugo rebuilds automatically during `hugo server` — run this for local testing
- Language switching is automatic via URL structure; no special template logic needed for navigation
- Static files in `static/` are copied as-is to `public/` root
- Consider adding CI checks for broken links or Hugo build failures before suggesting major refactors
