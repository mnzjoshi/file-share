# ATLAS SCRIPT AGENT — OWNER APPROVAL PACK: ENVIRONMENT

**Decisions covered:** SA-001, SA-002, C1 (inspection), C2 (installation), supply-chain risk acknowledgement
**Architecture baseline:** Stage 2.3 (`revisions/20261116-stage-2.3/`, unchanged by this document)
**Document status:** **DRAFT — FOR OWNER REVIEW. NO DECISION RECORDED. NOTHING VERIFIED.**
**Prepared:** 18 November 2026, by Opus (documentation only)

> **No approval in this document is effective until the owner explicitly records it** in the decision fields and signature block below. A blank field, an unticked box or silence is **not** approval. Every item is **PENDING** until then.

This pack consolidates the earlier SA-001/SA-002 approval drafts and the corrections from their reviews into one self-contained document. It does not refer back to any earlier draft for content.

---

## 0. Sources and how this pack relates to Stage 2.3

| Source | Used for |
|---|---|
| `revisions/20261116-stage-2.3/OWNER_DECISIONS.md` §0, SA-001/OD-01, SA-002/OD-02 | Option definitions (preserved below verbatim in meaning), dependencies, the separate download approval |
| `revisions/20261116-stage-2.3/SECURITY_ARCHITECTURE.md` §4, §6.2, §7.1 | Path guard, ancestor check, interpreter fingerprint scope |
| `revisions/20261116-stage-2.3/ATLAS_SCRIPT_AGENT_TECHNICAL_ARCHITECTURE.md` §8-D, §10 | No agent-run Git; P0-P3 sequence |
| `revisions/20261116-stage-2.3/RUNTIME_EVIDENCE_REGISTER.md` RE-05 | The interpreter fingerprint the runtime must later support |
| `revisions/20261116-stage-2.3/IMPLEMENTATION_BACKLOG.md` §1a | P1-P3 prerequisites and restrictions |

No earlier environment-approval draft exists as a file in the architecture workspace; the earlier drafts were supplied in conversation. Their content is consolidated here.

**What this pack adds to Stage 2.3, without changing it:** exact paths, the NuGet runtime method, the eleven-package lock, and the C1/C2 split. Stage 2.3 records that SA-002 option B "needs a package download" whose approval names "the exact packages and versions, the index they come from, and the expected hashes". C1 and C2 below are that approval, split into an inspection step and an installation step. Stage 2.3 also lists numpy and Pillow under option B for the renderer; this pack **defers** them to P5, which narrows the download and does not change the option.

These decisions are to be recorded in this `owner_decisions/` folder, outside the immutable Stage 2.3 revision. Stage 2.3's `OWNER_DECISIONS.md` and its manifest are not edited.

---

## 1. Summary of the decisions requested

| # | Decision | Recommendation | Status |
|---|---|---|---|
| 1 | SA-001 — placement, code root, workspace root, Git | Option A | **PENDING** |
| 2 | SA-002 — interpreter | Option B | **PENDING** |
| 3 | C1 — download and static inspection only | Approve | **PENDING** |
| 4 | Supply-chain residual-risk acknowledgement | Owner's choice | **PENDING** |
| 5 | C2 — runtime extraction, environment creation, package installation | Decide only after C1 evidence is complete | **PENDING** |
| 6 | P1-P3 commencement | Follows from 1, 2, 3 (inspection complete), 4 and 5 | **PENDING** |

**Order of operations:** SA-001 and SA-002 → C1 approved → C1 inspection carried out and evidence recorded (§10 and the evidence manifest) → supply-chain acknowledgement → C2 decided → owner-run setup → P1-P3.

---

## 2. SA-001 — Placement, code root, workspace root, Git

### 2.1 Registered options (from Stage 2.3, meaning unchanged)

| Option | Description |
|---|---|
| A | Code root `G:\atlas_script_agent\` and workspace `G:\atlas_script_agent_workspace\`, both outside `G:\jarvis`. A local Git repository in the code root, with no remote |
| B | As A, with a private remote for backup |
| C | Code inside `G:\jarvis` (rejected by design in Stage 2.3) |
| D | As A, without Git (the manifest only) |

**Recommendation: Option A.**

### 2.2 Exact proposed locations

| Purpose | Path | Belongs to |
|---|---|---|
| Agent source code (Git repository, no remote) | `G:\atlas_script_agent\` | SA-001 |
| Workspace and project material (source texts, runs, packages) | `G:\atlas_script_agent_workspace\` | SA-001 |
| Runtime installation root | `G:\atlas_script_agent_runtime\` | SA-002 |
| Static inspection (C1) | `G:\atlas_script_agent_runtime\inspection\` | C1 |
| Inspection copy of the NuGet archive (C1) | `G:\atlas_script_agent_runtime\inspection\nuget\` | C1 |
| Inspection copies of the eleven wheels (C1) | `G:\atlas_script_agent_runtime\inspection\wheels\` | C1 |
| Runtime download staging | `G:\atlas_script_agent_runtime\downloads\` | C1 |
| Dedicated Python installation (C2) | `G:\atlas_script_agent_runtime\Python312\` | C2 |
| Dedicated virtual environment (C2) | `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\` | C2 |
| Dependency lock (version-controlled) | `G:\atlas_script_agent\environment\requirements.lock` | C2 |

None of these paths has been checked for existence, permissions, free space or reparse points. That is done by the owner under §2.4 before any of them is created or used.

### 2.3 Conditions of Option A

1. Code, workspace and runtime all remain **outside `G:\jarvis`**.
2. No ancestor of the workspace, the code root or the runtime root may contain `.claude\`, `CLAUDE.md`, `CLAUDE.local.md` or `.mcp.json` (Stage 2.3 security §6.2; confirmed at runtime by RE-03).
3. Directory permissions, junctions, symbolic links and other reparse points are checked before creation or use (§2.4). No proposed directory may redirect into a protected project or any unintended location.
4. The agent never invokes Git. The owner performs every Git operation and commit manually.
5. The local repository has **no remote**.
6. **No remote means no off-machine Git backup.** The owner arranges a separate backup of `G:\atlas_script_agent\` (and, separately, decides whether the workspace is backed up, since it holds source texts).

### 2.4 Directory-boundary and reparse-point check (owner-run, before creation or use)

For each path in §2.2, including each leaf directory (a child can be a junction or symbolic link even when its parent is not), and each of its ancestors up to the drive root, the owner checks and records in the evidence manifest (§E0):

| Check | Pass condition |
|---|---|
| Ancestor configuration files | none of `.claude\`, `CLAUDE.md`, `CLAUDE.local.md`, `.mcp.json` in any ancestor |
| Existence before creation | the path either does not exist, or exists and is empty and owned by the owner |
| Reparse points | neither the path nor any ancestor is a junction, symbolic link, mount point or other reparse point |
| Final-path resolution | the fully resolved path equals the written path (no redirection) |
| Permissions | the owner's account has the intended access; no unexpected inherited write access for other accounts |
| Protected-root separation | the resolved path is not under `G:\jarvis`, `G:\jarvis-stage3`, `G:\atlas_format_lab`, `G:\atlas_script_agent_audit`, `G:\atlas_script_agent_architecture`, the YCIS location, or any D1/D1.1 or Stage 4 H0 location |

**Hard STOP** if any path redirects into `G:\jarvis`, protected material or an unintended destination, or if an ancestor contains a forbidden configuration file. Reassess the location; do not work around it.

### 2.5 SA-001 owner decision

- [ ] APPROVE SA-001 **Option A**, with the conditions in §2.3 and the check in §2.4.
- [ ] Select another registered option (B / C / D), as defined in Stage 2.3: __________
- [ ] DEFER.

Owner conditions: ____________________________________________

**Status: PENDING**

---

## 3. SA-002 — Interpreter

### 3.1 Registered options (from Stage 2.3, meaning unchanged)

| Option | Description |
|---|---|
| A | Run under the engine interpreter `G:\jarvis\shortform_agent_engine\.venv\Scripts\python.exe`, read-only, with `-I -B` |
| B | A dedicated interpreter outside `G:\jarvis` (a new venv in or next to the code root), pinned to the same versions. Needs a package download, separately approved |

**Recommendation: Option B.** It keeps the agent's interpreter, its fingerprint and its `.pth` exposure independent of the engine environment (Stage 2.3 `OWNER_DECISIONS.md`, SA-002 trade-off table).

### 3.2 Proposed runtime baseline for Option B

| Field | Value |
|---|---|
| Package | `python` |
| Version | `3.12.10` |
| Distribution | Official CPython NuGet package (an archive, extracted; no installer) |
| Architecture | Windows x64 |
| Archive | `python.3.12.10.nupkg` |
| Package page | `https://www.nuget.org/packages/python/3.12.10` (NOT YET VERIFIED) |
| Download endpoint | `https://www.nuget.org/api/v2/package/python/3.12.10` (NOT YET VERIFIED) |
| Extraction target, after C2 only | `G:\atlas_script_agent_runtime\Python312\` |
| Base interpreter, after C2 only | `G:\atlas_script_agent_runtime\Python312\tools\python.exe` |
| Virtual environment, after C2 only | `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\` |

**Why not the traditional Windows installer.** Python 3.12.10 is already installed per-user on this machine (`C:\Users\MSI\AppData\Local\Programs\Python\Python312\`; the engine environment is built on it). Running `python-3.12.10-amd64.exe` again would typically enter maintenance mode (Modify / Repair / Uninstall) for that existing installation rather than create a second copy, and could change the interpreter the engine depends on. **The traditional installer must not be run under this approval.** The NuGet archive is extracted instead, so no installer, PATH change, launcher registration, file association or Windows installer maintenance operation takes place.

**Version qualification.** Python 3.12.10 is the **pinned Stage 2.3 architecture baseline** (the versions verified in the engine environment, ADR §14 Q22). It is **not** a statement that 3.12.10 is the latest maintained 3.12 release; later 3.12 maintenance releases exist. Any interpreter-version change requires a separately approved dependency and runtime revision (§11).

### 3.3 SA-002 owner decision

- [ ] APPROVE SA-002 **Option B**, with the runtime baseline in §3.2. (This does **not** approve any download; see C1 and C2.)
- [ ] Select Option A, as defined in Stage 2.3.
- [ ] DEFER.

Owner conditions: ____________________________________________

**Status: PENDING**

Approving SA-002 Option B without C1 and C2 provides no interpreter, so it does not by itself allow P1-P3 to start.

---

## 4. C1 — Preliminary download and static inspection authorisation

C1 exists to collect the evidence C2 needs. It permits **downloading, hashing, extracting into the inspection folder, listing and signature inspection**. It permits **no execution and no installation**.

### 4.1 What C1 authorises (and nothing beyond it)

**A. NuGet archive**
1. Download the exact archive `python.3.12.10.nupkg` from the download endpoint in §3.2 into `G:\atlas_script_agent_runtime\downloads\`.
2. Retrieve the official NuGet V3 service index and the registration and catalog metadata for `python` 3.12.10 (§4.2).
3. Verify the archive's SHA-512 (§4.2).
4. Extract a copy of the archive into `G:\atlas_script_agent_runtime\inspection\nuget\` (the archive in `downloads\` is kept unchanged for C2).
5. Inventory every file and directory, with sizes and SHA-256 values.
6. Read package metadata (`.nuspec`, `.dist-info` files) as text.
7. Inspect Authenticode signatures of the files listed in §4.6 without executing them.

**B. Wheels**
8. For each of the eleven wheels in §7: retrieve its version-specific PyPI JSON metadata, download that exact wheel into `G:\atlas_script_agent_runtime\inspection\wheels\` from the metadata's URL (§4.5), and compute its SHA-256.

**C. Records**
9. Record every result in the inspection evidence manifest (`INSPECTION_EVIDENCE_MANIFEST_TEMPLATE.md`, completed as a dated copy).

### 4.2 NuGet hash provenance procedure

1. Retrieve the official NuGet V3 service index: `https://api.nuget.org/v3/index.json`.
2. Discover the registration resource from the service index. Do not hard-code a registration base URL.
3. Retrieve the registration index for package id `python` and locate the page and leaf for version `3.12.10`.
4. Follow that leaf's `catalogEntry` reference.
5. Read `packageHash` and `packageHashAlgorithm` from the catalog entry.
6. Require `packageHashAlgorithm` to be SHA512; anything else is a STOP.
7. Decode the Base64 `packageHash` to its 64-byte binary digest, then convert it to hexadecimal.
8. Compute the SHA-512 of the downloaded `.nupkg` locally.
9. Compare the two hexadecimal values exactly, and record the Base64 value, the hexadecimal value, the local value, the metadata URLs, the retrieval date and the result.

A flat-container `.sha512` sidecar may also be checked, but its availability must not be assumed. The catalog entry is the canonical source. If the official metadata cannot be retrieved, **STOP**. Never substitute a hash from another source.

**What this proves.** A match shows the downloaded archive has the bytes described by NuGet's published metadata. The archive and the metadata come from the same service, so this does **not** independently prove who published the package. The package's NuGet signature file (`.signature.p7s`) is **not** verified by this manual procedure, and the record must not claim that it was.

### 4.3 Required archive contents (hard STOP if any is missing)

- `tools\python.exe`
- `tools\python312.dll`
- `tools\Lib\venv\`
- `tools\Lib\venv\scripts\nt\python.exe`
- `tools\Lib\venv\scripts\nt\pythonw.exe`
- `tools\Lib\ensurepip\`

The two Windows venv launchers are mandatory: `venv` builds the environment's `Scripts\python.exe` from them. Their absence is a hard STOP even if `tools\Lib\venv\` exists.

### 4.4 Complete base package inventory and bundled pip

**`tools\Lib\site-packages\`** — inventory the entire directory, without assuming it holds only pip:
- every file and directory, with size and SHA-256;
- every `.dist-info` directory, with the package name and version from its metadata;
- every other importable module or component (`.py`, `.pyd`, `.pth`, package directories);
- anything unidentified or unexpected.

Every component is classified (expected / unexpected / unidentified) and becomes part of the approved base-interpreter inventory and fingerprint (Stage 2.3 security §7.1). **Any unexpected or unidentified component requires owner review and an explicit acceptance before C2.** Without that acceptance, C2 cannot be signed.

**`tools\Lib\ensurepip\_bundled\`** — expected wheel: `pip-25.0.1-py3-none-any.whl`. Record its filename, version and SHA-256. If it is absent, or a different version, **STOP** and request an amended approval. No replacement pip is obtained under this approval.

### 4.5 Wheel download controls

For each of the eleven wheels in §7:
1. Retrieve `https://pypi.org/pypi/{project}/{version}/json`.
2. In its `urls` array, find the object whose `filename` exactly equals the approved wheel filename.
3. Take the download URL from that object.
4. Require scheme HTTPS and host `files.pythonhosted.org`; anything else is a STOP.
5. Download that exact file into `G:\atlas_script_agent_runtime\inspection\wheels\`.
6. Compute its SHA-256 locally.
7. Compare three values: the PyPI-published `digests.sha256`, the proposed lock value (§7), and the local value. All three must agree; otherwise STOP and owner review.

The wheels are for hashing and inspection only. They are not installed, not unpacked into any environment, not imported and not executed.

### 4.6 Authenticode verification of the source binaries (C1)

Check, on the extracted inspection copy, without executing them:
1. `tools\python.exe`
2. `tools\python312.dll`
3. `tools\Lib\venv\scripts\nt\python.exe`
4. `tools\Lib\venv\scripts\nt\pythonw.exe`

Each must carry a valid Authenticode signature identifying the **Python Software Foundation** as signer, with a valid certificate chain. Record for each: path, SHA-256, signature status, signer identity, chain result and verification date/time. **A missing, invalid or unexpected signature on any of the four is a hard STOP.**

**What this proves.** Publisher-signature evidence for these four files only. It does not authenticate the other files in the archive, and it does not replace the SHA-512 comparison.

### 4.7 What C1 does not authorise

- Extraction into `G:\atlas_script_agent_runtime\Python312\`.
- Executing, importing or loading any extracted file or wheel content.
- Creating any virtual environment.
- Installing any package.
- Downloading anything other than the one NuGet archive, its metadata, the eleven wheels and their PyPI metadata.
- Running the agent, a provider, the renderer or a model.
- Any change to `G:\jarvis`, SCALE DROP (`G:\atlas_format_lab`), YCIS, or the existing `C:` Python installation.

Nothing in `inspection\` may ever be executed, including after C2. The C2 runtime comes from the archive in `downloads\` (§5.1).

### 4.8 C1 owner decision

- [ ] APPROVE C1: download and static inspection only, as defined in §4.1-§4.7.
- [ ] DO NOT APPROVE.
- [ ] DEFER.

Owner conditions: ____________________________________________

**Status: PENDING**

---

## 5. C2 — Final runtime extraction, environment creation and installation

C2 is a separate decision. It may be signed **only after** the complete C1 evidence (§10) is recorded and accepted, and the supply-chain acknowledgement (§9) is decided.

### 5.1 Re-verification immediately before extraction

Immediately before extracting into `G:\atlas_script_agent_runtime\Python312\`:
1. Recompute the SHA-512 of the **same** archive file inspected under C1 (`downloads\python.3.12.10.nupkg`).
2. It must equal the catalog digest recorded under C1. **If not, STOP.**
3. Do not substitute a fresh download. A new download restarts C1 and requires a new C2.

### 5.2 Runtime extraction and environment creation (owner-run)

1. Extract the re-verified archive into `G:\atlas_script_agent_runtime\Python312\`.
2. Optionally confirm that the extracted `tools\` file hashes equal the C1 inventory.
3. Create the virtual environment at `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\` using `G:\atlas_script_agent_runtime\Python312\tools\python.exe`, with system site-packages **not** included.
4. `venv` installs pip into the environment from the bundled `ensurepip` wheel. This is a local operation and needs no PyPI download. If pip is not available from the bundled wheel, STOP; do not fetch pip.
5. Record: base Python version and architecture; bundled pip wheel filename, version and SHA-256; the environment's pip version (expected `25.0.1`); the environment's interpreter path; `include-system-site-packages = false`; confirmation that no package from the engine environment or the `C:` installation is visible. Any discrepancy is a STOP pending review.

### 5.3 Generated launcher checks (hard STOP on mismatch)

After the environment is created, check:
- `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\Scripts\python.exe`
- `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\Scripts\pythonw.exe`

Each file's SHA-256 must equal its source launcher in the inspected archive (`tools\Lib\venv\scripts\nt\python.exe` and `pythonw.exe` respectively), and each must carry a valid Python Software Foundation Authenticode signature. Equality of the bytes is the expected `venv` behaviour on Windows, but it is **not yet verified** on this distribution; a mismatch is a STOP for review, not something to explain away.

### 5.4 Package installation

Installation follows §8 exactly, with the lock in §7.

### 5.5 C2 owner decision

Evidence attached and accepted (all required; see §10):
- [ ] NuGet archive evidence
- [ ] Contents, site-packages inventory and bundled pip evidence
- [ ] Authenticode evidence for the four source binaries
- [ ] Eleven-wheel three-way hash evidence
- [ ] Directory-boundary and reparse-point evidence
- [ ] Supply-chain acknowledgement decided (§9)

Decision:
- [ ] APPROVE C2: runtime extraction (§5.1-§5.2), environment creation and launcher checks (§5.3), and installation of exactly the eleven locked packages (§7-§8).
- [ ] APPROVE subject to these amendments: __________
- [ ] DO NOT APPROVE.
- [ ] DEFER.

Owner conditions: ____________________________________________

**Status: PENDING**

---

## 6. Hard-stop conditions (consolidated)

| # | Condition | Stage |
|---|---|---|
| S1 | A proposed path or ancestor holds `.claude\`, `CLAUDE.md`, `CLAUDE.local.md` or `.mcp.json` | §2.4 |
| S2 | A path or ancestor is a junction, symlink or other reparse point, or resolves somewhere else, especially into `G:\jarvis` or protected material | §2.4 |
| S3 | NuGet metadata cannot be retrieved from the official service | C1 §4.2 |
| S4 | `packageHashAlgorithm` is not SHA512, or the SHA-512 values differ | C1 §4.2 |
| S5 | Any required archive item in §4.3 is missing, including either venv launcher | C1 §4.3 |
| S6 | An unexpected or unidentified component in `site-packages` without explicit owner acceptance | C1 §4.4 |
| S7 | Bundled pip wheel absent or not 25.0.1 | C1 §4.4, C2 §5.2 |
| S8 | A wheel URL that is not HTTPS on `files.pythonhosted.org`, or a wheel whose three hashes do not all agree | C1 §4.5 |
| S9 | A missing, invalid or unexpected Authenticode signature on any of the four source binaries | C1 §4.6 |
| S10 | The archive's SHA-512 at C2 differs from the C1 record | C2 §5.1 |
| S11 | Environment creation fails, pip is not obtainable from the bundled wheel, or the environment sees outside packages | C2 §5.2 |
| S12 | A generated launcher's hash differs from its source launcher, or its signature fails | C2 §5.3 |
| S13 | pip's resolver needs any package outside §7, any hash check fails during installation, or the installation wrapper exits with an error | C2 §8 |
| S14 | Smart App Control or another Windows protection blocks a file or an import | C2 §8 |

At any STOP: record the event in the evidence manifest, change nothing further, and return to the owner. **Never disable, bypass or weaken a Windows protection mechanism.**

---

## 7. Complete eleven-package lock

The complete approved project dependency closure. The SHA-256 values are **proposed lock values, NOT YET VERIFIED**; C1 §4.5 verifies them.

| Package | Version | Exact wheel filename | Proposed lock SHA-256 |
|---|---|---|---|
| pydantic | 2.14.1 | `pydantic-2.14.1-py3-none-any.whl` | `9195d967ec791692a04438115466764fb8b9a27b31f14a760437694f40d6b454` |
| pydantic-core | 2.50.1 | `pydantic_core-2.50.1-cp312-cp312-win_amd64.whl` | `132529c83901437ff642f585216831bf5fd7a91df66829907e155192ead62498` |
| annotated-types | 0.8.0 | `annotated_types-0.8.0-py3-none-any.whl` | `f072f4d804ea359e4eaf198b1af7a8b0943881a87f31bb764f8bf219bb9419e0` |
| typing-extensions | 4.16.0 | `typing_extensions-4.16.0-py3-none-any.whl` | `481caa481374e813c1b176ada14e97f1f67a4539ce9cfeb3f350d78d6370c2e8` |
| typing-inspection | 0.4.4 | `typing_inspection-0.4.4-py3-none-any.whl` | `65b8397ba37ccbce054456aaccddfc91e6e3083c92824df348d96ca832f3f147` |
| jsonschema | 4.26.0 | `jsonschema-4.26.0-py3-none-any.whl` | `d489f15263b8d200f8387e64b4c3a75f06629559fb73deb8fdfb525f2dab50ce` |
| attrs | 26.1.0 | `attrs-26.1.0-py3-none-any.whl` | `c647aa4a12dfbad9333ca4e71fe62ddc36f4e63b2d260a37a8b83d2f043ac309` |
| jsonschema-specifications | 2025.9.1 | `jsonschema_specifications-2025.9.1-py3-none-any.whl` | `98802fee3a11ee76ecaca44429fda8a41bff98b00a0f2838151b113f210cc6fe` |
| referencing | 0.37.0 | `referencing-0.37.0-py3-none-any.whl` | `381329a9f99628c9069361716891d34ad94af76e461dcb0335825aecc7692231` |
| rpds-py | 2026.9.1 | `rpds_py-2026.9.1-cp312-cp312-win_amd64.whl` | `5ce8943f79c2210f7abcc28e86367b03b28d95027fd01c46d2472373ae70c86f` |
| PyYAML | 6.0.3 | `pyyaml-6.0.3-cp312-cp312-win_amd64.whl` | `5fcd34e47f6e0b794d17de1b4ff496c00986e1c83f7ab2fb8fcfe9616ff7477b` |

**Basis for the versions.** On 17 November 2026 the author read (read-only) the installed package metadata of the existing engine environment (`G:\jarvis\shortform_agent_engine\.venv`, Python 3.12.10). All eleven versions above are installed there, and their declared requirements close over this set: pydantic 2.14.1 requires exactly `pydantic-core==2.50.1`; the other requirements are minimum versions satisfied by the listed versions. This establishes the version set and its closure. It does **not** verify any filename or hash.

**PyPI hash provenance (C1 §4.5).** For each wheel, the PyPI-published `digests.sha256` for the exact filename, the proposed lock value and the locally computed value must all agree. A mismatch is a STOP; the lock is corrected and C2 re-approved. A PyPI hash match establishes integrity relative to PyPI's published metadata. It does **not** independently establish the publisher's authenticity.

**Explicitly excluded:** NumPy; Pillow; renderer-specific packages (deferred to the P5 approval); GPU libraries; additional AI or model packages; any package not in this table.

---

## 8. Lock file and installation command

**Lock location (version-controlled):** `G:\atlas_script_agent\environment\requirements.lock`. Not under the workspace. The lock lists exactly the eleven packages, each pinned with `==` and its approved `--hash=sha256:` value.

**Interpreter that runs pip:** the virtual environment's `G:\atlas_script_agent_runtime\venvs\atlas-script-agent\Scripts\python.exe` — never the extracted base interpreter.

**pip configuration isolation.** In pip 25.0.1, `--isolated` on its own does **not** disable every configuration file. It skips the per-user configuration file and ordinary `PIP_*` environment-variable settings, but pip still loads the **global** (machine-wide) configuration file and the **site** (virtual-environment) configuration file, either of which could add a package source. pip skips all configuration files only when the `PIP_CONFIG_FILE` environment variable equals the null device, which on Windows is exactly `nul` (Python's `os.devnull`). pip reads `PIP_CONFIG_FILE` even in isolated mode. Both controls are therefore used together.

(Basis: the configuration loader `pip/_internal/configuration.py` in `pip-25.0.1-py3-none-any.whl`, as reported by the owner's reviewer and read as text by the author from the copy bundled with the existing `C:` Python 3.12.10 installation. pip was not executed. The behaviour of the pip installed under C2 is NOT YET VERIFIED.)

**Installation wrapper (owner-run in Windows PowerShell, after C2):**

```powershell
$previousPipConfigFile = [Environment]::GetEnvironmentVariable(
    'PIP_CONFIG_FILE', 'Process'
)

try {
    $env:PIP_CONFIG_FILE = 'nul'

    & 'G:\atlas_script_agent_runtime\venvs\atlas-script-agent\Scripts\python.exe' `
        -m pip --isolated install `
        --no-deps `
        --require-hashes `
        --only-binary=:all: `
        --index-url https://pypi.org/simple `
        -r 'G:\atlas_script_agent\environment\requirements.lock'

    if ($LASTEXITCODE -ne 0) {
        throw "pip installation failed with exit code $LASTEXITCODE"
    }
}
finally {
    if ($null -eq $previousPipConfigFile) {
        Remove-Item Env:PIP_CONFIG_FILE -ErrorAction SilentlyContinue
    }
    else {
        $env:PIP_CONFIG_FILE = $previousPipConfigFile
    }
}
```

The wrapper saves the process-level `PIP_CONFIG_FILE`, sets it to `nul` for this one command, runs the approved virtual-environment interpreter, stops on a non-zero exit code, and restores the previous value in the `finally` block. The value must be exactly `nul`; pip compares it to `os.devnull` as a string. The pip arguments are the same as in earlier drafts of this approval; only the configuration control is added. A failure of the wrapper is STOP S13.

| Control | Function |
|---|---|
| `PIP_CONFIG_FILE=nul` | Prevents pip from loading any configuration file: global, user and virtual-environment (site) |
| `--isolated` | Ignores ordinary `PIP_*` environment-variable settings and the user configuration file. On its own it does **not** disable the global or site configuration files |
| `--no-deps` | Installs only what the lock lists; nothing is pulled in as a dependency |
| `--require-hashes` | Every file must match a hash in the lock, or installation fails |
| `--only-binary=:all:` | Wheels only; no source distributions and no local builds |
| `--index-url https://pypi.org/simple` | Specifies the single approved index. With no configuration file loaded and environment variables ignored, nothing adds another source |

Because `--no-deps` is used, the lock must contain the complete closure; §7 is that closure. No additional index, package, source distribution, version substitution or automatic upgrade is authorised, and pip itself is not upgraded.

**Windows protections.** The compiled modules in pydantic-core, rpds-py and PyYAML are not expected to carry individual Authenticode signatures. If Smart App Control or any other Windows protection blocks a file or an import, STOP and record it (S14). Never bypass or disable the protection.

After installation, record the installed package list and versions in the evidence manifest (§E5). It must equal §7 exactly.

---

## 9. Supply-chain residual-risk acknowledgement

The owner is asked to read and decide this separately. **Silence is not acceptance.**

The verification in this pack establishes the following, and no more:
1. The NuGet SHA-512 comparison shows the archive is consistent with the hash published by the NuGet service, which also served the archive.
2. The NuGet package signature (`.signature.p7s`) is **not** verified by the manual procedure.
3. Authenticode verifies the four named Python binaries (and, at C2, the two generated launchers). It does not authenticate every file in the archive, such as the standard library's `.py` files.
4. PyPI hash matching shows the downloaded wheels are the files PyPI's metadata describes. It does not independently prove who built or published them.
5. The compiled extension modules in pydantic-core, rpds-py and PyYAML do not necessarily carry individual Authenticode signatures.
6. Windows Smart App Control and other protections must not be bypassed; a block is a STOP.

**Owner decision:**
- [ ] ACCEPT the stated residual supply-chain risk for this environment.
- [ ] DO NOT ACCEPT: require stronger independent provenance or package-signature verification (for example NuGet repository-signature verification with a NuGet client, or PyPI attestations where published) before C2.

Owner conditions: ____________________________________________

**Status: PENDING**

---

## 10. Evidence required before C2

The completed, dated copy of `INSPECTION_EVIDENCE_MANIFEST_TEMPLATE.md` must contain:

| Evidence | Manifest section |
|---|---|
| Directory-boundary, ancestor, permission and reparse-point results for every §2.2 path | E0 |
| NuGet: URL, filename, size; catalog `packageHash` (Base64) and `packageHashAlgorithm`; hexadecimal digest; local SHA-512; metadata URLs and date; result | E1 |
| Full relative-path archive inventory with sizes and SHA-256; presence of every §4.3 item | E2 |
| Complete `site-packages` inventory and classification; owner acceptance of any unexpected item | E2 |
| Bundled pip filename, version, SHA-256 | E2 |
| Authenticode results for the four source binaries | E3 |
| For each of the eleven wheels: metadata URL, download URL, filename, published SHA-256, lock SHA-256, local SHA-256, result | E4 |

The manifest's environment section (E5) is completed **after** C2 setup, not before.

---

## 11. Python security-update review (operational requirement)

1. Review the pinned runtime and the eleven packages for security advisories and maintenance releases **quarterly**, and additionally whenever a relevant advisory is published.
2. Record each review (date, sources consulted, findings, decision) beside these decisions.
3. A runtime or package version change requires all of: owner approval; a revised dependency lock; a revised runtime manifest; repeated interpreter and launcher verification (C1/C2 equivalents); and reassessment of the tool and renderer fingerprints (Stage 2.3 security §7.1; RE-05).
4. Nothing is upgraded silently, including pip.

Python 3.12.10 remains the baseline unless the owner approves a formal revision.

---

## 12. P1-P3 gates and restrictions

P1-P3 may start only when **all** of these are recorded:
1. SA-001 Option A approved.
2. SA-002 Option B approved.
3. C1 approved **and** the C1 inspection complete, with its evidence recorded.
4. The supply-chain acknowledgement decided as ACCEPT (or stronger verification completed).
5. C2 approved, and the C2 setup completed without a STOP.

Stage 2.3 also requires the acceptance-id supersession table to be read before tests are written (backlog SA-011).

Restrictions during P1-P3 (Stage 2.3 backlog §1a, unchanged):
- No provider execution. No renderer execution. No model or live AI-service calls.
- Only the authorised owner-run environment setup and the test suite's own subprocesses; AC-SAF-15 (audit-hook subprocess test) is permitted as a test-suite subprocess.
- Build the renderer-free `text_cards` test format alongside SCALE DROP in P3.
- Git operations are manual and owner-controlled; the agent never invokes Git.
- No change to `G:\jarvis`, SCALE DROP, YCIS or other protected material.

**Not approved by this pack:** SA-003 (provider); OD-07 (child-process file access); OD-08 (creative scope and budget); OD-13 (Process Monitor); the real-provider spike; P4-P7; renderer integration or execution; NumPy, Pillow or renderer dependencies; production deployment or publishing.

---

## 13. Owner approval record

**No approval is effective until explicitly recorded below.**

Owner: ______________________________

| Decision | Decision taken | Date | Initials |
|---|---|---|---|
| SA-001 (§2.5) | PENDING | | |
| SA-002 (§3.3) | PENDING | | |
| C1 download and static inspection (§4.8) | PENDING | | |
| C1 inspection completed, evidence recorded (§10) | PENDING | | |
| Acceptance of any unexpected `site-packages` component (§4.4) | PENDING / not applicable | | |
| Supply-chain residual-risk acknowledgement (§9) | PENDING | | |
| C2 extraction, environment creation and installation (§5.5) | PENDING | | |
| C2 setup completed without a STOP (§5-§8) | PENDING | | |
| P1-P3 commencement (§12) | PENDING | | |

Conditions: ____________________________________________

Signature: ______________________________   Date: ______________

**Final status: PENDING OWNER DECISION. Nothing in this pack has been verified, downloaded, extracted, executed or installed.**
