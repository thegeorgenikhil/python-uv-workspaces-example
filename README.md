# Python Monorepo Template with `uv`

This repository demonstrates how to structure a **Python monorepo** using [`uv`](https://github.com/astral-sh/uv) — a fast, modern Python package manager. It organizes services and shared logic into a clean, scalable architecture.

## 🗂 Project Structure

```
.
├── README.md
├── pyproject.toml       # Root workspace configuration
├── uv.lock              # uv lock file for reproducible installs
├── apps/                # Application services
│   ├── server/          # Main server app
│   │   ├── main.py
│   │   └── pyproject.toml
│   └── another-server/  # Another simple app
│       ├── main.py
│       └── pyproject.toml
└── lib/                 # Shared libraries
    ├── logger/          # Logging utility using loguru
    │   ├── pyproject.toml
    │   └── src/logger/
    │       ├── __init__.py
    │       └── py.typed
    └── db/              # Dummy database interface
        ├── pyproject.toml
        └── src/db/
            ├── __init__.py
            └── py.typed
```

## 🚀 Getting Started

### 1. Initialize the Root Workspace

```bash
uv init
```

This creates a `pyproject.toml` file. You can delete the generated `main.py` as we will structure apps separately.

### 2. Create Application Services

Use the `--app` flag to initialize individual applications under `apps/`:

```bash
uv init --app apps/server
uv init --app apps/another-server
```

This also updates the root `pyproject.toml` to include the new apps as workspace members.

### 3. Create Shared Libraries

Use the `--lib` flag to create reusable libraries under `lib/`:

```bash
uv init --lib lib/logger
uv init --lib lib/db
```

Again, this updates the workspace to include these libraries.

### 4. Add External Dependencies

#### For a Library

To add `loguru` to the `logger` library:

```bash
uv add loguru --package logger
```

#### For an App

To add `colorama` to the `server` app:

```bash
uv add colorama --package server
```

### 5. Add Internal Library Dependencies

To use your own libraries within an app:

```bash
uv add logger --package server
uv add db --package server
```

This links the internal libraries as dependencies in the app.

### 6. Run an App

Run apps directly via `uv`:

```bash
uv run apps/server/main.py
uv run apps/another-server/main.py
```

Alternatively, run via `python`:

```bash
python apps/server/main.py
```

If module resolution fails, make sure your libraries use the correct structure (`src/` layout) and are properly defined in the `pyproject.toml`.

## 📌 Notes

* All libraries should use the **[src layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)** (`src/<pkgname>/`) for clean imports.
* `uv` automatically handles workspace dependency resolution and caching, making it ideal for monorepos.
* The `py.typed` file enables type-checkers like MyPy or Pyright to recognize type hints from libraries.

## 🧪 Example Behavior

* `apps/server/main.py` logs a message and simulates a DB call.
* `apps/another-server/main.py` logs a message only.
* Both apps share common logic via `lib/logger` and `lib/db`.

## 🧰 Requirements

* Python ≥3.8
* [`uv`](https://github.com/astral-sh/uv) installed (`pip install uv` or follow official guide)

---
## 🧠 Tips

### 🔄 Syncing All Packages in the Monorepo

To install **all dependencies** for every app and library in the workspace (not just the current one), use:

```bash
uv sync --all-packages
```

This is especially useful when:

* Setting up a **new environment** (e.g. in CI or on a new machine)
* You've **cleared your virtual environment** or `.venv`/`__pypackages__` folder
* You want to **ensure everything is fully in sync** with the `uv.lock` file

Without `--all-packages`, `uv sync` only installs dependencies for the **current package** you're working in.