# AGENTS.md - Connected Boxes

# OPENCODE INSTRUCTIONS - READ FIRST
> You are an expert Hugo developer. This file contains the mandatory constraints for this repository.
> Before taking any action, you must verify that your proposed plan adheres to the **Safety & Restrictions** section below.

## Stack
- Hugo static site (extended), hugo-book theme as submodule
- TOML config in `hugo.toml`
- Content: blog posts, about page. No tests or linting configured.

## Standard Hugo Directory Structure
Reference: https://gohugo.io/getting-started/directory-structure/

```
project/
├── archetypes/      # Templates for new content (e.g., default.md)
├── assets/          # Global resources (images, CSS, JS) processed by asset pipeline
├── content/         # Markdown content files (protected - see Safety section)
├── data/            # Data files (JSON, TOML, YAML, XML) for content/config
├── i18n/            # Translation tables for multilingual sites
├── layouts/         # Templates for transforming content into HTML
├── public/          # Build output (auto-generated)
├── resources/       # Cached asset pipeline output (auto-generated)
├── static/          # Static files copied to public (favicon, robots.txt, images)
├── themes/          # Theme subdirectories
└── hugo.toml        # Project configuration
```

### Directory Purposes
| Directory | Purpose |
|-----------|---------|
| `archetypes/` | Content templates with front matter |
| `assets/` | Global resources for asset pipeline |
| `content/` | **PROTECTED** - All markdown content files |
| `data/` | Site data (YAML/JSON/TOML) |
| `i18n/` | Internationalization/translations |
| `layouts/` | HTML templates, use `_partials/` for theme overrides |
| `static/` | Static files (images, CSS, JS, favicon) |
| `themes/` | Theme submodules |

## Commands
| Command | Purpose |
|---------|---------|
| `hugo server` | Dev server with live reload |
| `hugo server -D` | Dev server showing drafts |
| `hugo --minify` | Production build (output: `public/`) |

## Theme (submodule)
```bash
git submodule update --init --recursive   # after clone
```
- Override theme: put files in project root `layouts/` (not in `themes/`)
- Theme dir: `themes/hugo-book/`

## Content
- Front matter uses TOML (`+++`)
- Images go in `static/`, reference with absolute path from site root
- Template: `archetypes/default.md`
- Site base URL: `https://ramanbhalla.com/`

## CI/CD
- `deploy.yml` is **disabled** (`.disabled` suffix). Enable by renaming when ready.

## Build verification
- Check `public/` output after build
- No automated link checking or linting configured

## SAFETY & RESTRICTIONS

### Content Protection (HIGHEST PRIORITY)
- **NEVER** delete any `.md` files in `content/`
- **NEVER** modify, update, or edit any `.md` files in `content/`
- **NEVER** delete any directories under `content/`

### Configuration Protection
- **NEVER** modify `hugo.toml` unless explicitly requested for a specific parameter change
- If config changes are needed, ask user first

### GitHub Safety
- **NEVER** run any git commands that interact with remote repositories (git push, git pull, git clone from remote, git fetch)
- **NEVER** create, modify, or delete branches
- **NEVER** make commits or push changes
- Local git operations (git status, git diff, git log) are acceptable
- If remote interaction is needed, ask the user explicitly before proceeding

### Hugo Standard Practices
- Use `layouts/_partials/` for theme overrides (NOT `layouts/partials/`)
- Only modify files in `layouts/` and `static/` for customization
- Never modify files inside `themes/hugo-book/` directly
- Place images in `static/`, never in content folders
- Use TOML front matter (`+++`) for all content files

## Workflow Requirements

### Mandatory Safety Check
After completing **any** task, before responding to the user:
1. Re-read the **Safety & Restrictions** section above
2. Verify no changes violated any constraints
3. If a violation occurred, revert and report immediately
4. Include "Safety check: PASSED" in your final response

## Writing Style Guide

When assisting with blog post revisions:

### Voice & Tone
- Maintain first-person, conversational tone
- Keep the honest, reflective voice with personal learnings
- Use analogies to explain complex concepts

### Structure & Formatting
- Use consistent heading hierarchy (prefer ### for major sections, #### for sub-sections)
- Keep blockquotes (> ) for key takeaways or hook quotes at start
- Use --- for section breaks between major ideas

### Writing Improvements
- Break long sentences (target < 25 words)
- Use bullet lists for enumerated items
- Add a hook at the start (quote, question, or personal moment)
- End with a reflection question or conversational close
- Apply bold formatting consistently for key terms (first use only)
- Ensure each post gets a final edit pass before publishing

### Post Lifecycle
- Draft posts stay in `content/posts/` with `draft: true`
- Final edit pass before removing draft flag
- Verify all links work and images reference correctly
