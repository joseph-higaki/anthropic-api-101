# anthropic-api-101

Working through Anthropic's [Claude with the Anthropic API](https://anthropic.skilljar.com/claude-with-the-anthropic-api) course. The course uses Jupyter notebooks; this repo is the local scaffold for the venv, dependencies, and notebook artifacts.

## Layout

```
.
├── .env.example        # template for ANTHROPIC_API_KEY (copy to .env)
├── .gitignore
├── .venv/              # local virtual environment (not committed)
├── data/               # any sample/input data used by notebooks
├── notebooks/          # Jupyter notebooks (one per lesson)
├── pyproject.toml      # project metadata + explicit dependencies
├── src/                # reusable Python helpers, importable from notebooks
└── README.md
```

## Why these choices

- **`pyproject.toml`** instead of `requirements.txt`: PEP 621 is the modern, standard way to declare project metadata and dependencies in one place. Editable installs (`pip install -e .`) make anything in `src/` importable from the notebooks without `sys.path` hacks.
- **`venv`** instead of a global install: keeps the course's package versions isolated from the system Python and from other projects.
- **Registered Jupyter kernel**: lets you pick this venv from the kernel menu in JupyterLab/VS Code regardless of which Jupyter you launch from.
- **`.env` + `python-dotenv`**: keeps `ANTHROPIC_API_KEY` out of source control; notebooks load it with `load_dotenv()`.

## How this scaffold was built

Reproducing the project from scratch:

```bash
# 1. Create directory structure
mkdir -p notebooks src data

# 2. Create the virtual environment
python3 -m venv .venv

# 3. Upgrade pip inside the venv
.venv/bin/pip install --upgrade pip

# 4. Write pyproject.toml (see file in repo) and install in editable mode
.venv/bin/pip install -e .

# 5. Register this venv as a Jupyter kernel
.venv/bin/python -m ipykernel install --user \
    --name=anthropic-api-101 \
    --display-name "Python (anthropic-api-101)"

# 6. Create local secrets file from the template
cp .env.example .env
# then edit .env and paste your real ANTHROPIC_API_KEY
```

## Daily use

```bash
# Activate the venv (optional — you can also call .venv/bin/<tool> directly)
source .venv/bin/activate

# Launch JupyterLab
jupyter lab

# In the notebook, select kernel: "Python (anthropic-api-101)"
```

Inside a notebook, load the API key like this:

```python
from dotenv import load_dotenv
import os
from anthropic import Anthropic

load_dotenv()
client = Anthropic()  # reads ANTHROPIC_API_KEY from environment
```

## Versions installed at setup time

Initial install (2026-05-04) on Python 3.12:

| Package        | Version |
|----------------|---------|
| anthropic      | 0.97.0  |
| jupyter        | 1.1.1   |
| ipykernel      | 7.2.0   |
| python-dotenv  | 1.2.2   |

`pyproject.toml` uses lower-bound (`>=`) specifiers rather than pins, so `pip install -e .` will pick up newer compatible releases on a fresh setup. If you want a fully reproducible lock, run `.venv/bin/pip freeze > requirements.lock` and commit that.

## Adding a dependency

1. Add it to the `dependencies = [...]` list in `pyproject.toml`.
2. Re-run `.venv/bin/pip install -e .`.

## Removing the Jupyter kernel

If you ever delete this project:

```bash
jupyter kernelspec uninstall anthropic-api-101
```
