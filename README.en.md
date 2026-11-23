# EOStage Extension Hub (`eostage-hub`)

[简体中文](README.md) | English

The extension registry and on-demand distribution repository for [EOStage](https://github.com/hhanmj/EOStage), hosting runtime components, remote sensing / GEE knowledge plugins, and workflow skills.

## Repository Layout

- `registry.json` — Extension index (Schema v2) recording release URLs or relative `package` paths, platform constraints, byte sizes, and 64-char lowercase SHA-256 digests.
- `blocklist.json` — Revocation list to block broken or unsafe entries by `id` or `sha256`.
- `components/` — Runtime component notes (large archives are hosted in `v1.1.0` Releases rather than tracked as Git blobs).
- `plugins/` — Remote sensing & GEE knowledge plugin bundles (`.zip`).
- `skills/` — Standalone workflow prompt skill manifests (`.json`).

## Current Entries (16)

### Runtime Components (4 entries · 5 Release archives)

| ID | Version | Platforms | Description |
| --- | --- | --- | --- |
| `vertical-grid-egm96` | `1.0.0` | All platforms | NGA 15' global geoid grid (EPSG:5773) |
| `vertical-grid-egm2008` | `1.0.0` | All platforms | NGA 2.5' high-resolution global geoid grid (EPSG:3855) |
| `native-gis-runtime` | `1.0.0` | `Windows x64` / `macOS Apple Silicon` | Private GDAL / PROJ dynamic runtime & projection data (per-platform packages) |
| `compat-worker` | `1.0.0` | `windows-x86_64` | Isolated compatibility worker for Windows x64 |

### Knowledge Plugins (4 entries · All Platforms)

| ID | Version | License | Description |
| --- | --- | --- | --- |
| `eostage-eo-recipes-plugin` | `1.1.0` | `Apache-2.0` | EO & eco-hydrology analysis recipes (83 GEE Python + 16 local GeoTIFF) |
| `eostage-gee-cookbook-plugin` | `1.1.0` | `Apache-2.0` | GEE cloud computing architecture & methodology handbook (5 chapters) |
| `eostage-gee-practice-plugin` | `1.0.0` | `Apache-2.0` | GEE research practice & guardrail cards (10 topics) |
| `eostage-eo-methodology-plugin` | `1.0.0` | `Apache-2.0` | EO model validation, attribution review, and routing skills (4 items) |

### Workflow Skills (8 entries · All Platforms)

| ID | Version | Description |
| --- | --- | --- |
| `skill-alos2-stripmap-insar` | `1.0.0` | ALOS-2 PALSAR-2 L-band vegetated-terrain InSAR workflow |
| `skill-nisar-gslc-subset-prep` | `1.0.0` | NISAR L/S dual-frequency GSLC remote subset preparation |
| `skill-gacos-tropospheric-delay` | `1.0.0` | GACOS atmospheric delay correction pairing workflow |
| `skill-eo-agent-capability-routing` | `1.0.0` | EO agent intent and tool routing |
| `skill-eo-model-explanation-review` | `1.0.0` | EO model attribution and error explanation review |
| `skill-eo-model-transfer-evaluation` | `1.0.0` | Cross-region EO model transfer evaluation |
| `skill-eo-workbench-concept` | `1.0.0` | EO workbench design concepts |
| `skill-gee-practice-cards` | `1.0.0` | GEE research practice & engineering guardrail cards |

## Registry & Plugin Format

### `registry.json` Entry Schema

```json
{
  "id": "my-eo-plugin",
  "name": "Sample EO Plugin",
  "kind": "plugin",
  "version": "1.0.0",
  "author": "Author",
  "license": "Apache-2.0",
  "category": "insar-sar",
  "description": "Short summary of the plugin",
  "package": "plugins/my-eo-plugin-1.0.0.zip",
  "sha256": "<64-char-lowercase-sha256>",
  "size": 12345,
  "install_root": "plugins/my-eo-plugin",
  "platforms": [
    "windows-x86_64",
    "macos-aarch64",
    "macos-x86_64",
    "linux-x86_64",
    "linux-aarch64"
  ]
}
```

- Core fields: `id`, `name`, `kind` (`component` / `plugin` / `skill`), `version`, `package` (relative path or official Release HTTPS URL), `sha256`, `size`, and `platforms` (or per-platform `platform_packages`).
- The client verifies exact byte `size` and `sha256` before extraction and rolls back on failure.

### Plugin Bundle Layout & `plugin.json`

Plugin `.zip` archives use clean relative paths (no path traversal or OS metadata files):

```text
plugin.json
skills/<skill-id>/SKILL.md
skills/<skill-id>/references/*.md
```

Minimal `plugin.json`:

```json
{
  "id": "my-eo-plugin",
  "name": "Sample EO Plugin",
  "version": "1.0.0",
  "author": "Author",
  "license": "Apache-2.0",
  "discipline": "insar-sar",
  "description": "Short plugin description",
  "bundledSkills": [
    {
      "id": "my-skill",
      "name": "Sample Skill",
      "description": "When and how to trigger this skill",
      "path": "skills/my-skill/SKILL.md"
    }
  ]
}
```

### `blocklist.json` Format

```json
{
  "version": 1,
  "updated_at": "2026-11-09T07:42:12Z",
  "blocked": [
    {
      "id": "bad-plugin",
      "sha256": "<optional: target a specific bad hash>",
      "reason": "Short user-facing reason"
    }
  ]
}
```

## Mirrors & Client Usage

- **Dual-Mirror Sync**: Hosted on GitHub and automatically mirrored to GitCode (commits, tags, and `v1.1.0` Release assets) via `.github/workflows/sync-mirror.yml`; the desktop client fetches from GitCode first and automatically falls back to GitHub Releases when needed.
- **Client Entry Points**: In EOStage, open **Plugins & Skills** to sync remote skills/plugins or import local `.zip` bundles, or open **Settings -> Runtime & Tools** to install geoid grids and native GIS runtime components online or from local packages.

## Contributing & Reporting Issues

- **Submitting plugins or skills**: Open a Pull Request or Issue with an explicit open-source license and zero local paths, credentials, or malicious code. Maintainers review contents and SHA-256 digests before adding entries to the curated registry.
- **Reporting broken or unsafe entries**: Open a repository Issue with the plugin `id` and observed failure.
