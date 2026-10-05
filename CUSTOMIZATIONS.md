# Customizations

Personal fork of [TheoLeeCJ/SemIf-OpenJev](https://github.com/TheoLeeCJ/SemIf-OpenJev), tracking upstream, not contributing back. My changes live on the `mattb/main` branch; `master` stays clean for upstream comparison.

Adopted 2026-10-04. Security vetted at upstream commit `23cf1f39fc9534fe81437200959b6dfc7106e45a`. Later upstream commits are NOT vetted.

## Changes

- 2026-10-04: `CUSTOMIZATIONS.md`, `.github/workflows/sync-upstream.yml`: added by repo-adopt. Sync is fetch only (see below).

## Syncing upstream

The weekly Action only reports new upstream commits and the files they touch. It never merges or pushes. To take an update, review the upstream diff first, then:

```bash
git fetch upstream
git diff 23cf1f39fc9534fe81437200959b6dfc7106e45a..upstream/master --stat
git checkout master && git merge --ff-only upstream/master && git push origin master
git checkout mattb/main && git merge master
```

## License reminder

Upstream license: **MIT**. Permissive, adapt freely. Attribution required if you redistribute.

## Vetting conditions

Verdict: **CONDITIONAL** (4 stage scan, 2026-10-04: no malicious code or prompt injection found).

- Single maintainer, 18 days old, 29 unreviewed upstream PRs. Do not merge any PR without a diff review, especially API server additions (opens network ports).
- Re vet any upstream change touching pyproject.toml, requirements.txt, manifests/models.json, webgpu-demo/worker.js, webgpu-demo/vendor/, benchmarks/fetch_sources.py, or any new .sh, .pth, .pkl, .pt or .bin file.
- Work in a fresh virtual environment (container or VM preferred). Add the undeclared jinja2 dependency. Set HF_HOME to a dedicated folder. After the first model download set HF_HUB_OFFLINE=1.
- Keep revision pins and trust_remote_code=False. Never point --model at a repo not in manifests/models.json. Never use torch.load.
- Skip optional extras (mlx, llamacpp, exl3). If using the exl3 bridge verify the wheel SHA 256 first. llamacpp compiles C++ at install.
- Needs one CUDA GPU for the default path. No GPU is on record for this machine.
- Do not run benchmarks/fetch_sources.py unless the external eval data is needed. Run pytest and benchmarks/verify_published.py before trusting results.
