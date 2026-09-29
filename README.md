# layer-notebook-graph

A standalone marimo notebook exercising the cuGraph + cuML + PyG + graphistry GPU
libraries, provisioned into the workspace volume at deploy time, as a standalone
OpenCharly layer repo.

This is a **data-only layer** — no packages, no services, no baked DAGs. The
notebook demonstrates four GPU library families: cuGraph (RAPIDS graph analytics,
PageRank via the `nx-cugraph` backend pattern), cuML (RAPIDS ML, KMeans on
synthetic blobs), PyTorch Geometric (`GCNConv` forward pass on `cuda:0`), and
graphistry (GPU-accelerated graph visualization). Every cell is self-contained
synthetic data, so it runs cleanly even when the OSM DAG has not produced
`/workspace/tiles/work/` yet.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `notebook-graph` |
| Data | `data/notebooks` → `workspace` volume, dest `notebooks` |
| Notebook | `gpu-libraries-demo.py` |
| Packages | none |
| Service / port | none |

## How to use it

Compose the layer as a nested `candy:` list inside a named box body:

```yaml
versa:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-notebook-graph:v2026.239.1639'
```

The notebook lands in the workspace volume's `notebooks/` subdirectory at deploy
time.

## Layout

- `charly.yml` — the `notebook-graph:` candy entity (the `data:` mapping and the
  `check:` assertions).
- `data/notebooks/gpu-libraries-demo.py` — the marimo notebook.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-versa:versa` — the image that composes this data-only
  layer. This repo declares no `skill:` entity; the gap is tracked in
  [`opencharly/opencharly#291`](https://github.com/opencharly/opencharly/issues/291).
- `/charly-versa:notebook-osm` — the sibling notebook for the OSM/GTFS stack.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
