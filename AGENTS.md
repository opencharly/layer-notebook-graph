# AGENTS.md — layer-notebook-graph

Standalone candy repo for the `notebook-graph` layer — a standalone marimo
notebook exercising the cuGraph + cuML + PyG + graphistry GPU libraries,
provisioned into the workspace volume. The candy lives in `charly.yml` at the
repo root: the `data:` mapping and the `check:` assertions.

The repo declares **no `skill:` entity**; the owning guidance is the family skill
`/charly-versa:versa` (the gap is tracked in `opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `notebook-graph:` candy entity.
- `data/notebooks/gpu-libraries-demo.py` — the marimo notebook.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-versa:versa` — the owning (family) skill. The image that composes this
  data-only layer and the GPU library stack. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  the `data:` field, `plan:` step verbs incl. `check:`, and service
  declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; the runtime
  checks (with `context: [runtime]`) assert the provisioned workspace volume and
  the notebook file. The notebook must stay self-contained synthetic data — it
  reads no env vars and must not depend on the OSM DAG output.
- This is a data-only candy — it declares no packages and no `require:`. Do not
  add a runtime dependency to satisfy a check.

## Modify this repo

- The `check:` assertions pin the notebook path
  (`${VOLUME_CONTAINER_PATH:workspace}/notebooks/gpu-libraries-demo.py`);
  renaming the notebook means updating the `check:` in the same change.
- The notebook demonstrates the four GPU library families with the idiomatic
  accelerator patterns (e.g. `nx.pagerank(G, backend="cugraph")` since
  `cugraph.datasets` was removed in 26.4); keep them current.
- New behaviour claims belong in the `plan:` as an observable `check:` step.
- This repo has no `skill:` entity, so there is no projected corpus to mirror.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
