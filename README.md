# animus-plugin-registry

The canonical index of public [Animus](https://github.com/launchapp-dev/animus-cli) plugins.

## What this is

A flat JSON index at [`plugins.json`](./plugins.json) listing known public Animus plugin repositories with their installation hints. The Animus CLI fetches this index for `animus plugin search` and `animus plugin browse`.

The index is **not authoritative** — it's a discovery aid. Anyone can publish an Animus plugin to a public GitHub repo and use `animus plugin install <owner>/<repo>` without listing here.

## How to add your plugin

1. Publish your plugin to a public GitHub repo (the [animus-plugin-template](https://github.com/launchapp-dev/animus-plugin-template) is a good starting point).
2. Cut a tagged release with binaries for at least one platform. The template's `release.yml` uses cross-rs to produce all four supported triples in one shot.
3. Open a PR against [`plugins.json`](./plugins.json) adding your entry.
4. The schema lives at [`schema.json`](./schema.json) — validate your entry locally before opening the PR:

   ```bash
   npm install -g ajv-cli ajv-formats
   ajv validate -s schema.json -d plugins.json -c ajv-formats
   ```

### Example entry

```json
{
  "name": "animus-provider-myllm",
  "kind": "provider",
  "repo": "your-org/animus-provider-myllm",
  "latest_tag": "v0.1.0",
  "description": "MyLLM provider plugin for Animus",
  "homepage": "https://github.com/your-org/animus-provider-myllm",
  "license": "MIT",
  "stability": "alpha",
  "platforms": [
    "aarch64-apple-darwin",
    "x86_64-apple-darwin",
    "x86_64-unknown-linux-gnu",
    "aarch64-unknown-linux-gnu"
  ],
  "tags": ["provider", "myllm", "llm"],
  "install_hint": "animus plugin install your-org/animus-provider-myllm"
}
```

## Schema

The full JSON Schema (draft 2020-12) is at [`schema.json`](./schema.json). The required fields per entry are:

| Field | Description |
| --- | --- |
| `name` | Unique plugin name (typically matches the repo name). |
| `kind` | One of `subject_backend`, `provider`, `trigger_backend`. |
| `repo` | GitHub repository in `<owner>/<name>` form. |
| `latest_tag` | Latest released tag (e.g. `v0.1.0`). |
| `description` | Short human-readable description. |
| `license` | SPDX license identifier (e.g. `MIT`, `Apache-2.0`). |
| `install_hint` | Suggested CLI command to install this plugin. |

Optional fields: `homepage`, `stability` (`alpha` / `beta` / `stable`), `platforms`, `tags`.

## Stability

`stability: alpha` is the default — most plugins are alpha during v0.4.x. Promote to `beta` or `stable` as your plugin matures and its protocol surface stabilizes.

## License

MIT for the registry index itself. Each listed plugin sets its own license.
