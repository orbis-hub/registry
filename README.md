<p align="center"><img src="https://raw.githubusercontent.com/orbis-hub/orbis/main/brand/logo-dark.svg" alt="" width="64"></p>

# orbis registry

`index.json` is what every orbis hub's **module store** reads (default registry url: `https://raw.githubusercontent.com/orbis-hub/registry/main/index.json`). hubs can add more registries under settings, so anyone can run their own.

## list your module

**the short way:** open [orbis-hub.github.io/registry](https://orbis-hub.github.io/registry/), fill in the form at the bottom. it writes the entry, opens a prefilled issue, a bot checks your tarball and turns it into a pull request. we merge, done. the same page is the browsable catalogue.

**by hand:**

1. publish a release of your module with a `module.tgz` asset (the [module template](https://github.com/orbis-hub/module-template) ships a workflow that does this on `git tag vX.Y.Z`).
2. open a pull request adding an entry to `index.json`:

```json
{
  "id": "my-module",
  "name": "My Module",
  "description": "one sentence",
  "repo": "github:you/orbis-module-my-module",
  "latest": "0.1.0",
  "tags": ["productivity"],
  "author": "you",
  "icon": "sparkles"
}
```

- `id` must equal the `id` in your `module.json`.
- `latest` must be a published release tag `v<latest>` with a `module.tgz` asset. the hub downloads `https://github.com/<repo>/releases/download/v<latest>/module.tgz`; set `tarball` (with a `{version}` placeholder) if your asset lives elsewhere.
- `icon` is a [pixelarticons](https://pixelarticons.com) name.
- `deps` / `softDeps` (optional arrays of module ids) mirror your manifest so the store shows *needs …* / *works with …* before installing. hard deps are installed along automatically if they are in a registry.

the `validate` workflow checks the json shape and that the tarball url answers on every pull request. the `submission` workflow does the same for issues labelled `module-submission` and opens the pr itself. `pages` publishes `site/` + `index.json` to github pages, so `https://orbis-hub.github.io/registry/index.json` works as a registry url as well.

## rules

modules run inside people's hubs with full access. keep them honest: declare `permissions` truthfully, no telemetry without opt-in, no obfuscated code. entries that break this are removed.
