---
task: Configure Tailwind CSS in existing Django project
slug: 20260519-120000_configure-tailwind-django
effort: standard
phase: complete
progress: 10/10
mode: interactive
started: 2026-05-19T12:00:00Z
updated: 2026-05-19T12:01:00Z
---

## Context
User has a Django project at /Users/admin/Python/ with Tailwind v4 already installed via npm inside mysite/. The package.json is in mysite/ (not project root). No static directory exists. settings.py has STATIC_URL but no STATICFILES_DIRS. home.html has no CSS link or {% load static %}. Goal: wire Tailwind v4 so classes in templates compile to a served CSS file.

Key facts:
- Tailwind v4: uses `@import "tailwindcss"` (not old base/components/utilities directives)
- CLI binary at mysite/node_modules/.bin/tailwindcss
- BASE_DIR resolves to /Users/admin/Python/ (parent.parent of settings.py)
- Templates at /Users/admin/Python/templates/

## Criteria

- [x] ISC-1: static/css/ directory created at project root (BASE_DIR)
- [x] ISC-2: mysite/input.css exists with `@import "tailwindcss"` directive
- [x] ISC-3: mysite/input.css has @source pointing to templates directory
- [x] ISC-4: settings.py has STATICFILES_DIRS = [BASE_DIR / 'static']
- [x] ISC-5: package.json build script compiles input.css to static/css/output.css
- [x] ISC-6: package.json watch script runs Tailwind CLI in --watch mode
- [x] ISC-7: home.html has {% load static %} tag in head section
- [x] ISC-8: home.html has <link> tag referencing {% static 'css/output.css' %}
- [x] ISC-9: npm run build executes without errors from mysite/ directory
- [x] ISC-10: output.css is generated and non-empty after build (165 lines)

- [x] ISC-A1: django-tailwind pip package not used
- [x] ISC-A2: Existing settings.py entries not removed or broken
- [x] ISC-A3: node_modules location in mysite/ not changed

## Decisions

## Verification

- ISC-1 ✓: `ls static/css/` confirms directory exists with output.css
- ISC-2 ✓: mysite/input.css contains `@import "tailwindcss"`
- ISC-3 ✓: mysite/input.css contains `@source "../templates"`
- ISC-4 ✓: grep STATICFILES_DIRS in settings.py returns `STATICFILES_DIRS = [BASE_DIR / 'static']`
- ISC-5 ✓: package.json build script: `tailwindcss -i ./input.css -o ../static/css/output.css`
- ISC-6 ✓: package.json watch script: same + `--watch`
- ISC-7 ✓: home.html line 1 has `{% load static %}`
- ISC-8 ✓: home.html line 8 has `<link rel="stylesheet" href="{% static 'css/output.css' %}">`
- ISC-9 ✓: `npm run build` exited 0 with "Done in 16ms"
- ISC-10 ✓: output.css is 165 lines, starts with Tailwind v4 header
