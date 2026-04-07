# django-scaffold

An AI agent skill that scaffolds new Django projects following a consistent set of production-ready patterns.

<img width="1301" height="275" alt="image" src="https://github.com/user-attachments/assets/1faed8a2-52b8-495d-9414-a98dbac49906" />

## Installation

```bash
npx skills add dr-rompecabezas/django-scaffold-skill
```

This works with Claude Code, Codex, Cursor, and [40+ other agents](https://github.com/vercel-labs/skills).

Then invoke it inside your agent session:

```text
/django-scaffold
```

## What It Does

Given a directory with `uv init` done and Django installed, this skill asks 7 targeted questions and generates a complete, production-ready project structure — every file, configured correctly, with no placeholders.

### Assumed Starting Point

- You are already inside the project directory
- `uv init` has been run — `pyproject.toml` exists
- `.venv` is created and activated
- `django` (or `django` + `wagtail`) is already installed via `uv add`

### Question Flow

| # | Question | Options |
| - | -------- | ------- |
| Q1 | Project type | Django full-stack / Wagtail CMS / Django REST API |
| Q2 | Project name | Used for pyproject.toml, Railway service, and DB name |
| Q3 | Main app name | Primary local app beyond `users` (default: `core`) |
| Q4 | Celery + Redis? | yes / no |
| Q5 | Frontend interactivity? | HTMX / Alpine.js / both / neither *(full-stack only)* |
| Q6 | Authentication | django-allauth / allauth + Google OAuth / built-in auth |
| Q7 | Languages? | English only or multilingual list |

### What Gets Generated

**Root files:** `manage.py`, `pyproject.toml` (updated), `.env`, `.env.example`, `.gitignore`, `.pre-commit-config.yaml`, `railpack.json`, `railway.json`, `docker-compose.yml`, `package.json` *(full-stack only)*

**config/:** `settings/base.py`, `settings/local.py`, `settings/production.py`, `urls.py`, `wsgi.py`, `asgi.py`, `celery.py` *(if Q4=yes)*

**users/:** Custom User model, admin, migrations, test factories

**{app\_name}/:** Models, views, URLs, admin, migrations, tests scaffold

**templates/:** `base.html` with Tailwind CSS, HTMX, and Alpine.js wiring *(full-stack only)*

**.github/workflows/:** `ci.yml` with pre-commit + pytest

### Settled Conventions

| Area | Decision |
| ---- | -------- |
| Deployment | Railway |
| Media storage | S3 with presigned URLs (`django-storages[s3]`) |
| Error monitoring | Sentry SDK |
| CI/CD | GitHub Actions |
| Styling | Tailwind CSS v4 via npm (no config file needed) |
| Python | latest stable |
| Django | latest LTS (x.2 series) |
| Dependency management | `pyproject.toml` + `uv` |
| Local services | Docker Compose (Postgres, pgAdmin, Mailpit, Redis/Celery if needed) |
| Django itself | Runs directly in terminal — not containerized |

## Files

| File | Purpose |
| ---- | ------- |
| `skills/django-scaffold/SKILL.md` | Skill definition — question flow and generation checklist |
| `skills/django-scaffold/reference.md` | Authoritative templates and patterns for all generated files |

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgements

The interview questions style is inspired by [Cookiecutter Django](https://github.com/cookiecutter/cookiecutter-django), but with an AI-driven twist (directive but not deterministic) and far narrower focus.

Much of the developer experience tooling (e.g., pre-commit) is inspired by [Adam Johnson](https://adamj.eu/)'s books and blog posts on production Django patterns.
