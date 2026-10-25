# Space Quiz

Space Quiz is a dependency-free, single-page browser quiz about space. It includes ten multiple-choice questions, score tracking, answer feedback, keyboard-accessible controls, responsive styling, and light/dark theme support.

## Run locally

No build step or dependencies are required. Open `index.html` directly in a browser, or serve the repository with any static file server:

```text
file:///C:/path/to/space-quiz/index.html
```

For example, with Python installed:

```bash
python -m http.server
```

Then visit <http://localhost:8000>.

## Deployment

The `main` branch is deployed automatically to GitHub Pages by the workflow in `.github/workflows/pages.yml`.
