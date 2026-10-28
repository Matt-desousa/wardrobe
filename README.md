# Wardrobe

A web app (PWA) that identifies clothing from photos with an on-device ML classifier and
stores items in a digital wardrobe. Frontend only, hosted on GitHub Pages, no paid services.

See [DECISIONS.md](DECISIONS.md) for what was decided and what is still open.

## Layout

| Path | Purpose |
| --- | --- |
| `ml/` | ML pipeline: notebooks, export scripts, local data (gitignored) |
| `web/` | Vite + React + TypeScript PWA, runs ONNX models in the browser |
| `.devcontainer/` | Codespaces setup for Python (CPU PyTorch) and Node |
| `.github/workflows/pages.yml` | Builds `web/` and deploys to GitHub Pages |

`ml/` and `web/` only share the ONNX files and `manifest.json` produced by
`ml/scripts/export_onnx.py` (copied into `web/public/models/`).

## Development (Codespaces)

```bash
# ML
cd ml && jupyter lab            # notebooks/01_compare_backbones.ipynb
python scripts/export_onnx.py   # writes ONNX + manifest into web/public/models

# Web
cd web && npm run dev           # forwarded port 5173 (HTTPS, camera works)
```

Codespaces is for development only. Production is the static GitHub Pages build.
