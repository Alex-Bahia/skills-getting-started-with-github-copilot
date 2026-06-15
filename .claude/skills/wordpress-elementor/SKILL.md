# WordPress & Elementor Pro Development — Complete Skill Router

This document is a comprehensive procedural guide for production-grade WordPress and Elementor Pro development. It serves as a central router directing developers to specialized sub-files based on their specific task.

## Key Architecture

The router organizes development work into two main categories:

**Core Topics** (mapped to sub-files):
- Plugin scaffolding, child themes, and code placement → `scaffolding.md`
- PHP standards including sanitization and security → `php-standards.md`
- JavaScript/CSS standards and enqueue patterns → `js-css-standards.md`
- Elementor-specific patterns (widgets, Dynamic Tags, Loop Grids) → `elementor-patterns.md`
- WooCommerce integration and HPOS compatibility → `woocommerce.md`
- REST API endpoint development → `rest-api.md`
- Off-canvas UI patterns → `offcanvas-ui.md`
- Performance and accessibility checklists → `performance.md`

## Golden Rules (Mandatory)

1. **Native APIs first** — Prefer WordPress/Elementor core APIs over custom solutions
2. **Sanitize in, escape out** — Every input sanitized; every output escaped
3. **Prefix everything** — All functions, classes, hooks, and CSS use project-specific prefixes
4. **State placement explicitly** — Every code response declares exact file locations
5. **No hardcoded visuals in widgets** — All appearance controlled via Elementor editor panels, never hardcoded CSS/PHP
6. **Clarify only when necessary** — Ask questions only if missing information changes code output

## Default Tech Stack

| Component | Version | Notes |
|---|---|---|
| **WordPress** | 7.0+ (May 2026) | PHP 7.4 minimum; includes Real-Time Collaboration, AI Client APIs, and Abilities API |
| **PHP** | 8.3 recommended | 8.4/8.5 carry "beta support" labels |
| **Elementor** | 4.2+ (June 2026) | Atomic Editor is now stable/default; V3 `Widget_Base` remains production-safe for third-party widgets |
| **WooCommerce** | 10.8+ | HPOS enabled by default since 8.2 |

## Mandatory Output Format

Every code response must include:

```
📍 PLACEMENT — Exact file path
⚙️ REQUIRES — Version dependencies
💡 WHY THIS APPROACH — Single paragraph rationale
📋 CODE — Complete, deployment-ready code
🔧 INTEGRATION NOTES — Manual setup steps if needed
```

## Critical Widget Constraint: No Hardcoded Visuals

**Every visual property must be an Elementor control.** Never hardcode colors, typography, spacing, borders, shadows, backgrounds, or alignment.

**Required controls checklist** (apply to all custom widgets):
- Typography via `Group_Control_Typography`
- Colors via `COLOR` controls with `selectors`
- Spacing via responsive `DIMENSIONS`
- Backgrounds via `Group_Control_Background`
- Borders via `Group_Control_Border`
- Box/text shadows via group controls
- Hover states with `:hover` selectors
- Always use `add_render_attribute()` instead of manual HTML concatenation
