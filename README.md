# comfy-core

**comfy-core** is a tiny, metadata‑only package published on PyPI.
Its sole purpose is to expose the **ComfyUI core version** to Python’s dependency resolver so that other packages (e.g. plugins, API nodes, templates) can declare which ComfyUI versions they support.

> This package contains **no code** and is **not** ComfyUI itself.

---

## Why does this exist?

ComfyUI isn’t installed via PyPI, so third‑party packages can’t express “I work with ComfyUI X.Y.Z” using normal Python dependency metadata.
**comfy-core** fills that gap: it mirrors ComfyUI release versions, letting packages depend on ComfyUI compatibility via standard `install_requires`/`dependencies`.

---

## How it works

* For every ComfyUI release `X.Y.Z`, we publish **comfy-core `X.Y.Z`** to PyPI.
* Packages declare a dependency on `comfy-core` using an exact version or a range.
* On startup, ComfyUI ensures `comfy-core==<its own version>` is installed.
  Then tools like **pip** or **uv** will automatically select the latest compatible versions of dependent packages.

---

## For package authors

Declare your supported ComfyUI versions by depending on `comfy-core`:

**Broad range (recommended):**

```toml
# pyproject.toml
[project]
dependencies = [
  "comfy-core>=0.3.60,<0.4.0",  # works with ComfyUI 0.3.60 up to (but not including) 0.4.0
]
```

**Exact match (pinned):**

```toml
[project]
dependencies = [
  "comfy-core==0.3.60",
]
```

Use a range when your package supports multiple patch/minor releases of ComfyUI. Pin only when necessary.

---

## Automation

This repository’s CI mirrors ComfyUI releases:

* When ComfyUI publishes `X.Y.Z`, we publish **comfy-core `X.Y.Z`** to PyPI.
* No runtime logic is shipped here—just versioned metadata—so installs are fast and safe.

---

## Notes

* Pre‑releases of ComfyUI may map to the corresponding base version (e.g. `0.3.61.dev*` → `0.3.61`) for resolver compatibility.
* Do **not** import `comfy-core`. It provides metadata only.
