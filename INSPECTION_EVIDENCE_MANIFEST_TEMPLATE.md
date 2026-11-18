# INSPECTION EVIDENCE MANIFEST — TEMPLATE

**Companion to:** `ENVIRONMENT_APPROVAL_SA001_SA002_C1_C2_DRAFT.md`
**Status:** **TEMPLATE. NO RESULTS RECORDED. NOTHING VERIFIED.**

How to use it:
- Do not fill in this template. Copy it to `INSPECTION_EVIDENCE_MANIFEST_<YYYYMMDD>.md` in this folder and fill in the copy.
- Every result cell starts as `PENDING`. Replace it only with a value actually observed, and add the method used.
- Record a STOP as soon as it happens (section E6), then change nothing further.
- Sections E0-E4 are completed under C1. Section E5 is completed only after C2.

Inspected by: ______________________   Date started: ______________   Machine: ______________

---

## E0. Directory-boundary and reparse-point checks (approval §2.4)

| Path | Ancestor config files absent | Exists before creation? (empty / owner-owned) | Reparse point on path or ancestor | Resolved path equals written path | Permissions as intended | Outside protected roots | Method / tool | Result |
|---|---|---|---|---|---|---|---|---|
| `G:\atlas_script_agent\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_workspace\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\inspection\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\inspection\nuget\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\inspection\wheels\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\downloads\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\Python312\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\` | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `G:\atlas_script_agent\environment\` (lock folder) | PENDING | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| Drive root `G:\` | PENDING | n/a | PENDING | n/a | n/a | n/a | | PENDING |

Forbidden ancestor items: `.claude\`, `CLAUDE.md`, `CLAUDE.local.md`, `.mcp.json`. Any finding, or any redirection, is STOP S1/S2.

---

## E1. NuGet archive (approval §4.1-§4.2)

| Field | Value |
|---|---|
| Download URL used | PENDING |
| Saved as | `G:\atlas_script_agent_runtime\downloads\python.3.12.10.nupkg` (PENDING) |
| Size (bytes) | PENDING |
| Download date and time | PENDING |
| Service index URL | PENDING |
| Registration resource URL (as discovered) | PENDING |
| Registration leaf for 3.12.10 | PENDING |
| Catalog entry URL | PENDING |
| `packageHashAlgorithm` | PENDING (must be SHA512) |
| `packageHash` (Base64, as published) | PENDING |
| Published digest (hexadecimal, decoded) | PENDING |
| Local SHA-512 (hexadecimal) | PENDING |
| Flat-container `.sha512` sidecar (optional) | PENDING / not checked |
| Metadata retrieval date and time | PENDING |
| Match | PENDING |
| `.signature.p7s` verified? | **No — not verified by this procedure** |

---

## E2. Extracted package (approval §4.3-§4.4)

Inspection copy: `G:\atlas_script_agent_runtime\inspection\nuget\`

### E2.1 Required items

| Item | Present | SHA-256 | Result |
|---|---|---|---|
| `tools\python.exe` | PENDING | PENDING | PENDING |
| `tools\python312.dll` | PENDING | PENDING | PENDING |
| `tools\Lib\venv\` | PENDING | n/a | PENDING |
| `tools\Lib\venv\scripts\nt\python.exe` | PENDING | PENDING | PENDING |
| `tools\Lib\venv\scripts\nt\pythonw.exe` | PENDING | PENDING | PENDING |
| `tools\Lib\ensurepip\` | PENDING | n/a | PENDING |

### E2.2 Full archive inventory

Attach as a separate file: `INSPECTION_INVENTORY_<YYYYMMDD>.tsv`, one line per file: relative path, size in bytes, SHA-256. Record here:

| Field | Value |
|---|---|
| Inventory file name | PENDING |
| SHA-256 of the inventory file | PENDING |
| File count | PENDING |
| Total size (bytes) | PENDING |
| `.nuspec` package id and version (read as text) | PENDING |

### E2.3 `tools\Lib\site-packages\` inventory

| Item (file, directory or `.dist-info`) | Distribution name and version (if any) | SHA-256 / file count | Classification (expected / unexpected / unidentified) | Notes |
|---|---|---|---|---|
| PENDING | PENDING | PENDING | PENDING | |

(Add a row for every item. Unexpected or unidentified items need the owner's explicit acceptance below before C2.)

Owner acceptance of unexpected or unidentified items: [ ] Accepted   [ ] Not accepted   [ ] Not applicable
Owner initials: ______   Date: ______________

### E2.4 Bundled pip

| Field | Value |
|---|---|
| Expected filename | `pip-25.0.1-py3-none-any.whl` |
| Filename found | PENDING |
| Version | PENDING |
| SHA-256 | PENDING |
| Result | PENDING |

---

## E3. Authenticode, source binaries (approval §4.6)

| File | SHA-256 | Signature status | Signer | Chain result | Verified on (date/time) | Method / tool | Result |
|---|---|---|---|---|---|---|---|
| `tools\python.exe` | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `tools\python312.dll` | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `tools\Lib\venv\scripts\nt\python.exe` | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |
| `tools\Lib\venv\scripts\nt\pythonw.exe` | PENDING | PENDING | PENDING | PENDING | PENDING | | PENDING |

Expected signer: Python Software Foundation. Any other result is STOP S9.

---

## E4. Wheels (approval §4.5, §7)

Folder: `G:\atlas_script_agent_runtime\inspection\wheels\`

| Package | Version | PyPI JSON URL | Download URL (from metadata) | Exact filename | Published SHA-256 | Lock SHA-256 | Local SHA-256 | Result |
|---|---|---|---|---|---|---|---|---|
| pydantic | 2.14.1 | PENDING | PENDING | `pydantic-2.14.1-py3-none-any.whl` | PENDING | `9195d967ec791692a04438115466764fb8b9a27b31f14a760437694f40d6b454` | PENDING | PENDING |
| pydantic-core | 2.50.1 | PENDING | PENDING | `pydantic_core-2.50.1-cp312-cp312-win_amd64.whl` | PENDING | `132529c83901437ff642f585216831bf5fd7a91df66829907e155192ead62498` | PENDING | PENDING |
| annotated-types | 0.8.0 | PENDING | PENDING | `annotated_types-0.8.0-py3-none-any.whl` | PENDING | `f072f4d804ea359e4eaf198b1af7a8b0943881a87f31bb764f8bf219bb9419e0` | PENDING | PENDING |
| typing-extensions | 4.16.0 | PENDING | PENDING | `typing_extensions-4.16.0-py3-none-any.whl` | PENDING | `481caa481374e813c1b176ada14e97f1f67a4539ce9cfeb3f350d78d6370c2e8` | PENDING | PENDING |
| typing-inspection | 0.4.4 | PENDING | PENDING | `typing_inspection-0.4.4-py3-none-any.whl` | PENDING | `65b8397ba37ccbce054456aaccddfc91e6e3083c92824df348d96ca832f3f147` | PENDING | PENDING |
| jsonschema | 4.26.0 | PENDING | PENDING | `jsonschema-4.26.0-py3-none-any.whl` | PENDING | `d489f15263b8d200f8387e64b4c3a75f06629559fb73deb8fdfb525f2dab50ce` | PENDING | PENDING |
| attrs | 26.1.0 | PENDING | PENDING | `attrs-26.1.0-py3-none-any.whl` | PENDING | `c647aa4a12dfbad9333ca4e71fe62ddc36f4e63b2d260a37a8b83d2f043ac309` | PENDING | PENDING |
| jsonschema-specifications | 2025.9.1 | PENDING | PENDING | `jsonschema_specifications-2025.9.1-py3-none-any.whl` | PENDING | `98802fee3a11ee76ecaca44429fda8a41bff98b00a0f2838151b113f210cc6fe` | PENDING | PENDING |
| referencing | 0.37.0 | PENDING | PENDING | `referencing-0.37.0-py3-none-any.whl` | PENDING | `381329a9f99628c9069361716891d34ad94af76e461dcb0335825aecc7692231` | PENDING | PENDING |
| rpds-py | 2026.9.1 | PENDING | PENDING | `rpds_py-2026.9.1-cp312-cp312-win_amd64.whl` | PENDING | `5ce8943f79c2210f7abcc28e86367b03b28d95027fd01c46d2472373ae70c86f` | PENDING | PENDING |
| PyYAML | 6.0.3 | PENDING | PENDING | `pyyaml-6.0.3-cp312-cp312-win_amd64.whl` | PENDING | `5fcd34e47f6e0b794d17de1b4ff496c00986e1c83f7ab2fb8fcfe9616ff7477b` | PENDING | PENDING |

The lock column holds the **proposed** values from the approval §7 (not yet verified). Each download URL must be HTTPS on `files.pythonhosted.org`. All three hashes must agree, or STOP S8.

---

## E5. Environment (after C2 only; approval §5-§8)

| Field | Value |
|---|---|
| C2 approval recorded on | PENDING |
| Archive SHA-512 recomputed immediately before extraction | PENDING |
| Equals E1 published digest | PENDING |
| Extracted to | `G:\atlas_script_agent_runtime\Python312\` (PENDING) |
| Extracted `tools\` hashes equal E2.2 (optional) | PENDING / not checked |
| Base interpreter version and architecture | PENDING |
| venv path | `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\` (PENDING) |
| `include-system-site-packages` | PENDING (must be false) |
| pip source (bundled wheel filename and SHA-256) | PENDING |
| venv pip version | PENDING (expected 25.0.1) |
| No package from the engine environment or `C:` installation visible | PENDING |

### E5.1 Generated launchers

| Generated file | SHA-256 | Equals source launcher (E2.1) | Signature status | Signer | Result |
|---|---|---|---|---|---|
| `venvs\atlas-script-agent\Scripts\python.exe` | PENDING | PENDING | PENDING | PENDING | PENDING |
| `venvs\atlas-script-agent\Scripts\pythonw.exe` | PENDING | PENDING | PENDING | PENDING | PENDING |

### E5.2 Installation

| Field | Value |
|---|---|
| Lock file path | `G:\atlas_script_agent\environment\requirements.lock` (PENDING) |
| SHA-256 of the lock file | PENDING |
| Installation wrapper run (verbatim, approval §8) | PENDING |
| `PIP_CONFIG_FILE` set to exactly `nul` during installation | PENDING |
| `PIP_CONFIG_FILE` restored to its previous value afterwards | PENDING |
| Exit status | PENDING |
| Installed packages and versions (equal to approval §7, exactly) | PENDING |
| Any Windows protection event (Smart App Control or other) | PENDING |

---

## E6. STOP events and deviations

| Date/time | Stop id (S1-S14) or deviation | What was observed | Action taken | Owner decision |
|---|---|---|---|---|
| | | | | |

---

## E7. Sign-off

| Stage | Completed by | Date | Owner reviewed (initials) |
|---|---|---|---|
| E0-E4 (C1 evidence complete) | | | |
| E5 (C2 setup complete) | | | |
