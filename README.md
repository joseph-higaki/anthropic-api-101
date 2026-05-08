# anthropic-api-101

My scratch repo for Anthropic's [Claude with the Anthropic API](https://anthropic.skilljar.com/claude-with-the-anthropic-api) course.

## Setup

```bash
python3 -m venv .venv
.venv/bin/pip install -e .
.venv/bin/python -m ipykernel install --user \
    --name=anthropic-api-101 --display-name "Python (anthropic-api-101)"
cp .env.example .env   # then paste your ANTHROPIC_API_KEY and VOYAGE_API_KEY
```

Then `jupyter lab` and pick the `Python (anthropic-api-101)` kernel.

## Layout

- `notebooks/` — one notebook per lesson
- `src/` — helpers, importable from notebooks via the editable install
- `data/` — sample inputs
- `pyproject.toml` — deps live here, not in `requirements.txt`

## Notes

- Adding a dep: edit `pyproject.toml`, then `.venv/bin/pip install -e .`
- Loading the key in a notebook: `load_dotenv(); client = Anthropic()`
- Versions are `>=` not pinned. For a lockfile: `.venv/bin/pip freeze > requirements.lock`
- Tearing down the kernel: `jupyter kernelspec uninstall anthropic-api-101`
