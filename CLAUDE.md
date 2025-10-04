# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo-based static website for Procedere, a Swiss consulting firm specializing in workplace integration and occupational health management. The site is in German (de-CH) and uses the custom `wl-theme` (Waldluft Theme) maintained as a git submodule.

## Development Commands

### Building and Running

```bash
# Start local development server
hugo server

# Build the site for production
hugo

# Build with production environment variables
HUGO_ENVIRONMENT=production HUGO_ENV=production hugo
```

### Content Management

```bash
# Create new content from archetype (will be created as draft)
hugo new content/<path>/<filename>.md

# Example: Create new team member
hugo new content/team/name.md
```

### Theme Development

The theme is a git submodule at `themes/wl-theme`. When working with the theme:

```bash
# Initialize/update theme submodule
git submodule update --init --recursive

# Pull latest theme changes
cd themes/wl-theme && git pull origin main
```

## Architecture

### Content Structure

The site follows Hugo's hierarchical content organization:

- **Main service sections** (flyers): `content/flyer/`
  - `case_mgmt/` - Case Management services
  - `job_coaching/` - Job Coaching services
  - `workshop/` - Workshop and training services
  - `betr_gsmgmt/` - Occupational Health Management services

  Each section has an `_index.md` with metadata (title, intro text, weight) and multiple child pages describing specific aspects of the service.

- **Team profiles**: `content/team/`
  - Each team member has their own `.md` file with frontmatter defining:
    - `title`, `sort` (for ordering), `role`, `email`, `phone`
    - `edu` (education/qualifications array)
    - `core` (core competencies array)
  - Images stored alongside (e.g., `claudia.jpg`, `claudia_sofa.jpg`)

- **Static pages**: Root-level pages like `datenschutz.md`, `impressum.md`, `olten.md`, `stellen.md`

### Configuration

- **Main config**: `config.yaml`
  - German language (`languageCode: de-ch`)
  - Theme colors defined in `params.styles` (orange: #D9DF20, blue: #1382BB)
  - Logo and favicon paths
  - Uses relative URLs

- **Navigation**: `data/menu.yaml`
  - Menu items with `title`, `mobile_title`, and either:
    - `autosub`: Auto-generates submenu from section
    - `sub`: Manual array of page paths

- **Other data files**: `data/footer.yaml`, `data/references.yaml`, `data/strings.yaml`

### Theme & Layouts

- Custom theme: `themes/wl-theme/` (git submodule)
- Theme uses UIKit framework (`themes/wl-theme/assets/uikit/`)
- Project-specific layouts in `layouts/partials/` override theme defaults
- Specialized layouts for:
  - Homepage: `themes/wl-theme/layouts/index.html`
  - Service flyers: `themes/wl-theme/layouts/flyer/`
  - Team pages: `themes/wl-theme/layouts/team/`

### Assets

- Theme assets: `themes/wl-theme/assets/` (fonts, SCSS, images, UIKit)
- Project images: `assets/img/` (logo, favicon, etc.)
- Team member photos stored in `content/team/` alongside their markdown files

## Deployment

The site deploys to GitHub Pages via `.github/workflows/deploy-hugo.yaml`:
- Triggers on push to `main` branch and on pull requests
- Uses Hugo Extended v0.122.0
- Caches Hugo binary and resources directory
- Builds to `public/` directory
- Deploys artifact to GitHub Pages

## Hugo Version

This project requires **Hugo Extended v0.151.0** as specified in:
- `.tool-versions`: `hugo-extended 0.151.0`
- GitHub Actions workflow
- Theme minimum version: 0.107.0

## Content Authoring Notes

- All content is in German (Swiss German locale)
- Dates format: `Date: 2025-10-03`
- Line breaks in titles use `<br>` (e.g., "Case<br>Management")
- Draft content: Add `Draft: true` to frontmatter (from archetype template)
- Weight parameter controls ordering in lists
