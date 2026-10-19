# v1.0.0 — First binary preview

Released: 2026-10-20.

- Natural-language and reference-image part generation in the SolidWorks task pane.
- Local changes to existing parts, assembly tools, mechanism motion and bounded static simulation.
- Per-conversation workspaces and interactive, advisory delivery.
- First-use API configuration with eight provider/compatible endpoint choices. Requests are blocked until configured.
- Current-user Windows DPAPI protection for API keys; no system environment-variable setup.
- Bundled Python 3.12 and Node.js, compiled native DLLs and Python bytecode.
- Relative runtime paths, install/uninstall entry points and file integrity manifests.
- English/Chinese documentation, CAutoD branding and five OBS-derived GIF demos.

## Checks performed

The distribution was copied to an isolated directory containing Chinese characters and spaces. Checks cover compiled backend startup, empty-profile configuration gating, DPAPI storage, native DLL relative-path/environment handling, SDK/document access and an offline agent tool loop. Twenty-one relevant Python regression tests and frontend model/send tests passed. ZIP integrity and Windows ZIP-reader checks passed.

These checks did not call live model APIs or operate the user's active SolidWorks instance. A clean installation on a second computer and live validation of every provider remain pending. See RELEASE_CHECKS.json.

## Known limits

The native test environment is SolidWorks 2026 SP0.0. Other versions require validation. The application UI currently uses Chinese. API network access, quotas and tools support depend on the chosen service. Anthropic's native Messages protocol is not implemented.

Python bytecode and .NET DLLs are not encryption. Implementation source is omitted, but reverse engineering remains possible. The app release is 1.0.0; native DLL internal compatibility versions remain in the 0.4.2 series.

For upgrades, save work, close SolidWorks, unregister the old installation, extract the new one into a separate writable directory, and register it there. Keep backups of workspaces; never upload API settings or private designs to the public repository.
