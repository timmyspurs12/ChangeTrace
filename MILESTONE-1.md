# ChangeTrace Milestone 1: validation and NAS test guide

## Status

**PARTIAL PASS — ready for NAS validation, not runtime-validated**: the refined Markdown Skill passed the available creation-tool static checker. Fictional PDF fixture text was checked locally in the earlier milestone. The registered Uliya experts and a UGREEN NAS were not available; none of the seven workflow tests has been run. In particular, actual single-file enforcement, inspectable source attribution, expert handoffs, financial comparison, read-only runtime behavior, and the central POC objective remain unproven. A static pass and local PDF parsing do not prove NAS compatibility.

## Refinement: evidence and delegation hardening

1. Each document delegation now explicitly requests the documented `file` parameter with exactly one selected path and a nonempty required `query`, and includes the assigned role and applicable user identifiers. The child remains responsible for executing its own workflow. The Skill stops the dependent comparison if the child cannot execute the single-file request or its actual result cannot establish the effective source scope. The catalogue permits `file` but does not prove runtime enforcement.
2. Every material observation now requires document-expert identification of the selected source, returned inspectable evidence, and a clear association between that evidence and the observation. Preserve returned coverage warnings, failures and uncertainty. Missing or insufficient attribution means `unverified` and exclusion from matching, comparisons and arithmetic.
3. A document read now requires all three gates: filesystem delegation `completed`, actual stat result establishing successful access to the selected path, and the path remaining within the user-authorized scope. Completion alone is not access; stat is not proof of readability. User path, actual stat result and any explicitly returned resolved path remain distinct.
4. The four-file revision test now requires separate approval A and B source records, each with its selected path, actual evidence, amount, stated revision information and uncertainty. Never merge them or select a controlling approval from filename, modification time, amount, plausibility or assumed chronology; withhold a definitive comparison until human resolution.

These are authored instruction changes, **not behavioral validation**. The prior three-file boundary, explicit fourth-file test exception, read-only restrictions, financial safeguards and failure handling remain in effect. No workflow test or fixture parsing was rerun as part of this refinement.

## Skill and capability mapping

The only Skill package file is `skills/changetrace-poc/SKILL.md`. Fixtures and this guide are outside the Skill package and are not to be included in a Skill ZIP.

| Parent responsibility | Registered expert | Catalogue capability | Evidence / restrictions |
| --- | --- | --- | --- |
| Inspect each selected path separately | `file-agent` | `ug-filesystem` → `stat` | Required `path`; read; approval `none`. Keep user path, actual stat result, and any explicitly returned resolved path separate. |
| Analyze one selected document at a time | `document-agent` | `ug-document` → `qa` or `extract` | Both require `query`; repeatable `file` is optional in the catalogue but the Skill requires exactly one selected path as its sole `file` value. Read; approval `none`. Actual scope enforcement and per-fact citations are not guaranteed by the catalogue. |

Sources: supplied `UGREEN_Uliya_Capability_Catalog_v2.0.0.json`, entries `ug-filesystem.stat`, `ug-document.qa` and `ug-document.extract`; and `UGREEN_Uliya_Skill_Kit_0.7.3_Technical_Reference.txt`, `SKILL.md`, `references/expert-orchestration.md`, `references/completion-and-errors.md`, and `references/cli-usage-rules.md`. The parent assembles a self-contained delegation containing actual selected path, role and constraints; the registered child chooses its action. The Skill does not call a child CLI directly. This implementation does not use `reconcile`: it is optional and non-authoritative, and has not been tested.

## Static validation actually performed

The supplied attachments contained a consolidated reference and catalogue, not the checker executable. A third-party GitHub mirror of the kit archive was read into memory and extracted into `/tmp/changetrace-validator` for validation only. The archive SHA-256 was `cf557e5a022e4297892f39768132855154232efdedeb726b2845450ccd70e1b3`, matching the hash stated in the supplied technical reference; the archive catalogue was structurally identical to the supplied JSON. This establishes those matches, **not independent proof that the mirror is an official UGREEN download**. No checker scripts were placed in the Skill or repository.

An attempted checker `--help` call in the original milestone failed because the checker treats its sole argument as a target directory: `TARGET_NOT_FOUND`, one error, exit 1. This was a usage probe, not a Skill validation.

### Refinement validation

The updated Skill was checked with:

```text
node /tmp/changetrace-validator/uliya-skill-kit/scripts/check-skill.mjs skills/changetrace-poc
```

Actual checker output, exit status **0**:

```json
{
  "checkFormatVersion": "2.0.0",
  "status": "static-pass",
  "target": {
    "skillName": "changetrace-poc"
  },
  "catalog": {
    "checkedVersion": "2.0.0"
  },
  "files": [
    {
      "path": "SKILL.md",
      "sha256": "c47ba2dac2a301741980ebb25b1e0db0244dd3db16a393601c567a7b13694a34"
    }
  ],
  "summary": {
    "errors": 0,
    "warnings": 0,
    "info": 0
  },
  "diagnostics": []
}
```

`sha256sum skills/changetrace-poc/SKILL.md` independently returned `c47ba2dac2a301741980ebb25b1e0db0244dd3db16a393601c567a7b13694a34`.

### Original milestone validation

Before this refinement, the original Skill validation command was:

```
node /tmp/changetrace-validator/uliya-skill-kit/scripts/check-skill.mjs skills/changetrace-poc
```

Actual output, exit 0:

```json
{
  "checkFormatVersion": "2.0.0",
  "status": "static-pass",
  "target": { "skillName": "changetrace-poc" },
  "catalog": { "checkedVersion": "2.0.0" },
  "files": [{ "path": "SKILL.md", "sha256": "6981e85b37dc3f33b627049e5f619b26dbde286688f514e06730c8b477d0d607" }],
  "summary": { "errors": 0, "warnings": 0, "info": 0 },
  "diagnostics": []
}
```

A separate local, inline Python presence check passed for the two delegation targets, attribution gate, exclusion of unverified observations, three-file boundary, controlled fourth approval, read-only wording, all five delegation states, and no direct parent CLI invocation. These are text-presence checks only, not behavioral tests.

## Local fixture checks actually performed

Fixture creation and checking used `pypdf==6.4.0` (source: https://github.com/py-pdf/pypdf; license: BSD-3-Clause), installed temporarily under `/tmp/changetrace-validation-deps`; it is a local test dependency, not part of the Skill. No fixture-generation or validation script is included in the repository. The validation was run as inline Python using `PYTHONPATH=/tmp/changetrace-validation-deps python3 -` with `PdfReader(..., strict=True)`, text extraction and exact whitespace-normalized text assertions.

The first fixture-text check failed: a line break inside `higher-output` produced `higher- output` on extraction. The six valid synthetic PDFs were regenerated without breaking words at hyphens. A rerun then passed exact extracted-text checks on all six PDFs. The same local check rejected `approval-corrupt.pdf` with `PdfReadError: EOF marker not found`, and confirmed `missing-approval.pdf` does not exist. These checks establish behavior in local pypdf, not Uliya PDF parsing.

| Synthetic PDF | Exact-text local check | SHA-256 |
| --- | --- | --- |
| `request.pdf` | pass | `0b33c5a6649730ed9f4dde5453df758963409d587e512b974d612db0c173373b` |
| `approval-1200.pdf` | pass | `5b294f2b447c413c35c64b1cc89f1f743d26c2a61c2b9af6acd49bd729ac2919` |
| `invoice-1500.pdf` | pass | `efe8a062bd40a659c517605d0c4be3cc8a40b9a5c3f9c8f80bf5818bd26b1fa7` |
| `invoice-progress.pdf` | pass | `9fb4d39f2a347716e79c47a74cebb1e1eaa9964727bb00a0fe55fad8e9a1dc7c` |
| `approval-co02.pdf` | pass | `d34c351a387650199d301df681881f4b72fd1642cb473051f96c985ab6b5692d` |
| `approval-1400.pdf` | pass | `e393334e5e8cb56e56f9600a7cf7f47e3cd0fd95092441b06a5b24d1f5ee686b` |

## Seven workflow test results

All paths below refer to synthetic files under `test-fixtures/changetrace-poc/`. The Skill itself has not been installed or run, and no expert results exist.

| Test | Selection / intended outcome | Execution | Environment | Result | Evidence / observed failure | Limitation |
| --- | --- | --- | --- | --- | --- | --- |
| 1. Source-attributed $300 discrepancy | `request.pdf`, `approval-1200.pdf`, `invoice-1500.pdf`; only with inspectable evidence and comparable basis | Not run | No Uliya/NAS available | Blocked | No expert output; no workflow failure observed | Central attribution and arithmetic unproven |
| 2. Missing approval | Base request and invoice, nonexistent `missing-approval.pdf`; stop dependent comparison | Not run | No Uliya/NAS available | Blocked | Local filesystem confirms missing fixture; no expert output | NAS path-error behavior unknown |
| 3. Unreadable approval | Base request and invoice, `approval-corrupt.pdf`; report failure and withhold | Not run | No Uliya/NAS available | Blocked | Local pypdf rejected corrupt fixture; no expert output | Uliya parser/error behavior unknown |
| 4. Unclear financial basis | Base request and approval, `invoice-progress.pdf`; withhold calculation | Not run | No Uliya/NAS available | Blocked | Fixture text verified locally; no expert output | Model interpretation and withholding unproven |
| 5. Attribution unavailable | Three readable base files; if expert support is insufficient, mark unverified and withhold | Not run | No Uliya/NAS available | Blocked | No expert output; do not fabricate an attribution failure | May be evaluated only if actual output lacks sufficient support |
| 6. Conflicting identifier | Base request and invoice, `approval-co02.pdf`; withhold match and difference | Not run | No Uliya/NAS available | Blocked | Fixture text verified locally; no expert output | Match behavior unproven |
| 7. Conflicting revisions | Explicitly select base request, invoice, `approval-1200.pdf` and `approval-1400.pdf`; show distinct A/B records with source, evidence, amount and revision information; withhold definitive comparison | Not run | No Uliya/NAS available | Blocked | Both approval texts verified locally; no expert output | Four-file mode, source separation and revision handling unproven |

A correct numerical answer without inspectable source attribution for every material observation **fails Test 1 and the POC's central technical objective**. A user-provided path or a completed call is not source evidence.

## How to test on the assigned NAS

1. Use the organizer-provided import and connection instructions to install the Skill folder. Preserve the original test fixture files; upload copies to a private, authorized test location as setup. Do not include the fixtures in the Skill package. Do not use real project data or include credentials in a recording.
2. Run Test 1 with exactly the three base files selected individually. Ask: “Review these selected request, approval note and invoice for project HS-17 and change CO-01. Show source evidence for each observation and compare comparable amounts. Do not modify or save files.” Confirm that the paths in the instruction are the actual paths selected on that device, not guessed paths.
3. Record the actual delegation states and inspectable source evidence if the interface exposes them. Open each original and check every material observation. If an expert does not return adequate support, mark that field unverified and Test 1 failed; do not add a citation manually and call it a pass.
4. Run Tests 2–6 with the specified replacements. To test a nonexistent path, supply a missing path if the interface allows it; otherwise use a genuinely inaccessible authorized-test path and report what was actually possible. Test 5 is an observation of actual insufficient attribution, not permission to fabricate expert output.
5. For Test 7 explicitly select the four specified documents and explicitly request the controlled conflicting-revisions test. Do not search for additional approvals. Verify that approval A and approval B remain distinct records, each with selected path, actual source evidence, stated amount, any explicit revision information and uncertainty; verify that no controlling revision is chosen from filename, modification time, amount, plausibility or assumed chronology. The definitive invoice-versus-approved comparison must remain withheld pending human resolution.
6. Record actual model and device configuration, action states, output, warnings, failures, and corrections. If a method to compare fixture hashes before and after is available, use it to check byte changes; matching hashes alone do not prove no metadata or other state changed. Back up only permitted synthetic results before assigned NAS access expires.

## Known limitations

Source-level excerpts or locators, PDF handling, true delegation behavior, permission enforcement, comparison accuracy, completeness, data locality, and stability are unverified. An expert may return a plausible answer without sufficient evidence; the Skill must mark it unverified. No report is saved and no filesystem discovery, external integration or reconciliation is implemented. A static checker does not test the semantics of the instructions or execute the Skill.
