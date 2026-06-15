# Lucidity Coordination

This repo is the coordination layer for the Lucidity product family. It does
not contain the product code directly; each child directory remains its own Git
repo.

Start with [LUCIDITY.md](LUCIDITY.md) for the system map, repo boundaries, and
AI session bootstrap rules.

Open [lucidity.code-workspace](lucidity.code-workspace) to work across the
family from one editor window.

GalvoFS is tracked from this coordination repo as a submodule-style child repo:

```bash
git submodule update --init --recursive
```

The GalvoFS source of truth remains
[MichaelDanCurtis/GalvoFS](https://github.com/MichaelDanCurtis/GalvoFS).
