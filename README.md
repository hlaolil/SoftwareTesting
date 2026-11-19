# CSE 270 — Software Testing

My work for CSE 270 (Software Testing) at BYU–Pathway Worldwide. The system under test is the **Teton Chamber of Commerce** website, version 1.6, and the small Django service it talks to. The repository also runs the course's CI/CD pipeline in GitHub Actions.

**Site under test:** https://hlaolil.github.io/SoftwareTesting/teton/1.6/

## What's here

| Folder | What it is |
| --- | --- |
| `teton/1.6/` | The Teton Chamber of Commerce site: home page with business spotlights, business directory, a four-step join form, a thank-you page and an admin login |
| `directorydata_service/` | Django back end for the site. `/data/all` returns the business directory as JSON. `/users/` checks admin logins and `/users/ingest` receives join-form submissions |
| `tests/` | pytest tests, including a mocked request to the directory data endpoint |
| `.github/workflows/main.yml` | The pipeline (below) |

## The pipeline

Every push to `main` runs:

1. **Build and test:** sets up Python 3.12, lints with flake8 (syntax errors fail the build), starts a static server on port 5500 and runs `pytest`.
2. **Analyze:** CodeQL security scanning of the JavaScript.
3. **Deploy:** publishes the repository to GitHub Pages.

## Run it locally

```bash
# the Django service (the site calls it at http://127.0.0.1:8000)
cd directorydata_service
pip install django
python manage.py runserver

# the website, in another terminal
cd teton/1.6
python3 -m http.server 5500

# the tests, from the repository root
pip install pytest pytest-mock requests
pytest
```

The test cases and the defect report for the Teton site were managed separately in Squash TM.

> The admin credentials in `users/views.py` are deliberate test fixtures for the course. They are not real passwords.
