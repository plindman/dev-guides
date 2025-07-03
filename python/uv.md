# uv Cheat Sheet for Python Development

## ✨ Install

```bash
curl -Ls https://astral.sh/uv/install.sh | bash
# or
brew install astral-sh/uv/uv
```

## ⚙️ Init / Sync Project

```bash
uv init                # Initialize pyproject.toml and .venv
uv sync                # Sync deps from pyproject.toml
```

## 🚀 Add / Remove Packages

```bash
uv add <pkg>           # Add runtime dependency
uv add -d <pkg>        # Add development dependency
uv remove <pkg>        # Remove a package
```

## ▶️ Run 

```bash
uv run python main.py
uv run pytest
```

## 🔍 Help & Info

```bash
uv --help
uv add --help
```

## 🔗 Links

* Docs: [https://astral.sh/docs/uv](https://astral.sh/docs/uv)
* Repo: [https://github.com/astral-sh/uv](https://github.com/astral-sh/uv)
