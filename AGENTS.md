# Project guidance

- Run commands from the repository root: template, static and SQLite paths are relative to the working directory.
- First-time setup: `make setup` installs Python and pnpm dependencies and builds Tailwind CSS. Rebuild with `make build-css` after changing templates or `assets/css/source.css`; `app/static/css/main.css` is generated and ignored by Git.
- Python CI runs `make install`, `make build-css`, `make lint`, then `make test`. Run one test with `uv run pytest tests/test_app.py::test_course_crud`. The style test requires the CSS build.
- `mise run check` runs the repository's hk checks, including network-dependent Markdown link checks; `mise run setup` installs pnpm dependencies and Git hooks, not Python dependencies. Commit titles are checked against the conventional commit configuration.
- Tests delete `test.sqlite` and set `DATABASE_URL` before importing the app. The database is shared across a test run; create independent records rather than relying on test order. Use `TestClient` as a context manager to run startup.
- `app/main.py` registers the routers and initializes the database at startup. `app/database.py` creates tables and seeds data when no course exists; there are no migrations. The default database is `school.sqlite`; set `DATABASE_URL` before importing the app to override it.
- Routes render server-side templates through `app.dependencies.render`, which supplies CSRF tokens and consumes flash messages. Verify CSRF on POST forms; use POST for edit/delete and declare literal routes such as `/new` before `/{id}`.
