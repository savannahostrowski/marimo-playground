This repo hosts a sample Marimo notebook that I export as WASM-powered HTML (via Pyodide) and serve from a FastAPI app deployed to [FastAPI Cloud](https://fastapicloud.com). The notebook runs entirely in the browser — FastAPI just serves the static bundle. You can play around with it at [https://marimo-playground.fastapicloud.dev/](https://marimo-playground.fastapicloud.dev/).

## How it works

- [`playground.py`](playground.py) is the marimo notebook source.
- [`main.py`](main.py) is a tiny FastAPI app that mounts `output_dir/` as static files.
- [`output_dir/`](output_dir/) holds the generated WASM bundle. It's git-ignored but uploaded to FastAPI Cloud via [`.fastapicloudignore`](.fastapicloudignore).
- [`.github/workflows/fastapicloud-deploy.yml`](.github/workflows/fastapicloud-deploy.yml) runs the export and `fastapi deploy` on pushes to `main`.

## Local dev

```sh
uv run marimo export html-wasm playground.py -o output_dir --mode edit
uv run fastapi dev main.py
```

## Deploy

CI handles this on push to `main`. To deploy manually:

```sh
uv run marimo export html-wasm playground.py -o output_dir --mode edit
uv run fastapi deploy
```
