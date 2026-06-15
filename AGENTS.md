# Repository Guidelines

## Project Structure & Module Organization
This repository is a static single-page app. The main application lives in `index.html`, which contains the markup, CSS, and JavaScript in one file. GitHub Pages deployment is defined in `.github/workflows/pages.yml`; it copies `index.html` into `dist/` during CI. The `old/` directory contains retired material and should not be used for new work unless a change explicitly targets archived assets.

## Build, Test, and Development Commands
There is no package-based build step.

- `start index.html`
  Opens the app directly in the default browser on Windows.
- `python -m http.server 8000`
  Runs a local static server from the repository root for browser testing.
- `git status`
  Confirms the working tree before and after changes.

Deployment runs automatically on pushes to `main` through the Pages workflow.

## Coding Style & Naming Conventions
Follow the existing style in `index.html`:

- Use 2-space indentation in HTML, CSS, and JavaScript.
- Prefer `const` by default and `let` only when reassignment is required.
- Use `camelCase` for functions and variables, for example `renderBoard` or `safeJson`.
- Keep CSS class names descriptive and lowercase, using hyphenation when needed, for example `mode-switch`.
- Preserve the current single-file structure unless a task explicitly requires splitting assets.

## Testing Guidelines
There is no automated test suite yet. Validate changes manually in a browser:

- confirm the page loads without console errors
- exercise both train and airport modes
- verify import/export and local state persistence after reload

If you add logic with significant branching, include a short manual test note in the PR.

## Commit & Pull Request Guidelines
Current Git history uses short, imperative, lowercase subjects such as `add github pages workflow`. Keep commit messages concise and focused on one change.

Pull requests should include:

- a brief description of the user-visible change
- linked issue or task reference when available
- screenshots or short recordings for UI changes
- manual test notes covering the paths you checked
