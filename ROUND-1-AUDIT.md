# ChangeTrace — Round 1 submission audit

Checked against the official pages on 3 November 2026. This is a pre-submission audit, not a certification by UGREEN or legal advice. **No submission was made.**

## Submission artifacts

| Artifact | Size | SHA-256 |
| --- | ---: | --- |
| `submission/ChangeTrace-Project-Proposal.pdf` | 5,686 bytes | `51387d91d9275eb761fa44e90882a14d1b2e9b5eaaec7a7ad9ad90d4ba5114bd` |
| `submission/ChangeTrace-Pretest-Skill.zip` | 3,940 bytes | `975b6ce28eb54874fb0ccb0b91f5d5379dd5ca16b5b875c1af2455f61af8c0d1` |
| ZIP member `changetrace-poc/SKILL.md` | 10,318 bytes uncompressed | `c47ba2dac2a301741980ebb25b1e0db0244dd3db16a393601c567a7b13694a34` |

ZIP tree, as actually inspected:

```text
changetrace-poc/
└── SKILL.md
```

The archive contains **one file**, with no references, fixtures, audit documents, checker scripts, dependencies, manifests, executable scripts or other files. The member bytes match the checked working Skill. ZIP CRC validation passed; the member is not encrypted, a symlink, an absolute path or a traversal path. Python `zipfile.testzip()` returned `None`; `unzip -t` reported no errors. Total uncompressed size: **10,318 bytes**.

The proposal is a separate, unencrypted, two-page PDF. Strict local `pypdf` parsing and extracted-text comparison confirmed that all ten answer texts are present; rendered pages were inspected and text bounding boxes fit. The ten substantive answers total **340 words** under a conservative regex count that includes number fragments and treats hyphenated expressions as single words. Individual counts: 18, 25, 26, 30, 38, 27, 51, 36, 54, 35. Headings and development disclosure were excluded from the substantive-answer count. Local PDF rendering and extraction are not NAS tests.

## Working directory inventory before packaging

- `skills/changetrace-poc/SKILL.md`: latest and only Skill file, SHA-256 `c47ba2dac2a301741980ebb25b1e0db0244dd3db16a393601c567a7b13694a34`; no supporting references directory or linked references.
- `MILESTONE-1.md`: development validation and seven-test plan, SHA-256 `8d91e66d908b313d113292936c3ad0de0f706075fd9bcfb3208e5d0e8ccfd2e6`.
- `README.md`: repository heading only, SHA-256 `9bbddf3eb4d3e2b0b439a44a77616016a22e98c5f1cb0536ffce7afa0ef712ee`; not part of the Skill.
- `test-fixtures/changetrace-poc/`: six text-readable fictional PDFs and one deliberately corrupt PDF, outside the Skill. Their individual hashes are recorded in `MILESTONE-1.md`; corrupt fixture SHA-256 `a4b7ed1bb111d6c904dae9ce3dda9706f29daa0a73cb925af44e66687796fd98`. The missing-path case intentionally has no file.
- No duplicate Skill file was found. The old Skill hash in `MILESTONE-1.md` is clearly identified as the **original milestone validation**, not the current Skill; it is historical evidence, not a competing package file. No files were removed or changed to prepare the package.

## Static validation immediately before packaging

Command:

```text
node /tmp/changetrace-validator/uliya-skill-kit/scripts/check-skill.mjs skills/changetrace-poc
```

Exit status **0**; actual stdout:

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

The checked directory's Skill bytes were compared directly to the ZIP member after packaging. The available checker is from a **third-party mirror**, not independently authenticated as the official download. Its archive SHA-256 `cf557e5a022e4297892f39768132855154232efdedeb726b2845450ccd70e1b3` matches the archive hash asserted in the supplied technical reference, and its catalogue is structurally identical to the supplied JSON. The mirror source is https://github.com/sirmos/heirloom/blob/main/uliya-skill-kit-0.7.3-en.zip; its provenance and license were not independently verified. The official rules require using the official creation tool: obtain and check the official archive directly, or ask the organizer to confirm equivalence, before relying on this as proof of that requirement. The checked Skill itself was not modified for this submission preparation.

## Round 1 checklist

Official sources: [Devpost event and submission requirements](https://ugreenhackathon.devpost.com/), [Official Rules, especially sections 3 and 4](https://ugreenhackathon.devpost.com/rules), [UGREEN starter kit and Skill Creation Tool](https://ai.ugreen.com/pages/hackathon-starter-kit), and [SkillHub Content Submission and Publication Standards](https://api.ugnas.com/api/system/v1/ua/agreement/page?agreementCode=skillhub-publish-audit-spec&acceptLanguage=en-US).

| Requirement | Assessment and evidence |
| --- | --- |
| Separate proposal PDF, 10 answers, at most 500 English words, at most 1 MB | **Pass on inspected artifact**: two pages, all ten answers, 340 substantive words, 5,686 bytes. Proposal identifies the untested state and development aids. |
| Skill ZIP, ZIP and uncompressed total at most 10 MB, each file at most 1 MB, no more than 100 files | **Pass**: 3,940-byte ZIP, 10,318-byte uncompressed total, one 10,318-byte file; limits checked conservatively using decimal MB. |
| `SKILL.md` entry point with name, description, goal, inputs, capabilities, orchestration, completion/failure | **Pass by inspection and static checker**: valid frontmatter and required sections in `changetrace-poc/SKILL.md`. |
| Optional references are Markdown under `references/` and directly linked | **Not applicable**: none included or needed. |
| No separate manifest, scripts, executables, binaries or unsupported runtime dependencies | **Pass for archive contents**: the sole member is Markdown. No extra runtime dependencies or third-party components are packaged. UGREEN platform and its configured experts/model remain dependencies. |
| No credentials, personal data or unauthorized third-party material | **No issue found in the sole Markdown member** through manual inspection and a basic credential-pattern scan; ownership and authorization require entrant confirmation. This is not a guarantee that every possible secret pattern was detected. |
| Registered experts and documented capabilities | **Pass as documented design**, not runtime proof: `file-agent` / `ug-filesystem.stat`; `document-agent` / `ug-document.qa` or `extract`. Child experts choose and execute workflows; parent delegates. |
| Official UGOS Skill Creation Tool and authoritative documentation | **Needs confirmation**: supplied reference/catalogue and hash-matched mirror checker used, but direct origin of the mirror was not independently authenticated. The starter-kit page says the official tool must be used. |
| Disclosure of AI tools, third-party components, sources and licenses, and data flows | **Addressed, subject to entrant review**: Skill discloses Arena.ai Agent Mode for drafting/review and no bundled third-party components; proposal discloses local development aids and their known sources/licenses, and states mirror origin/license unverified. The Skill requests no external transfer; actual model configuration and data handling remain unknown. |
| Originality, ownership, team consent, entrant eligibility and terms | **Entrant must confirm**: cannot be established from files. Rules require eligible individuals or teams of up to three, qualifying new work, and necessary rights. The official rules list Nigeria among eligible countries, but actual residence and individual eligibility are not known here. |
| Round 1 NAS test, demo video, repository link | **Not required** on official event/rules pages; no video was prepared and no runtime was claimed. Confirm the current submission form and any newer official instructions before filing. |
| SkillHub safety/publication standards | **Preliminary review only**: read-only and explicit-scope instructions are present; actual permission, data flows, security and functional behavior cannot be verified here. The publication standards state that claimed compatibility must be genuinely installable and functional, while the contest rules expressly allow a non-operational Round 1 pre-test; do not represent this artifact as a proven publishable Skill. Seek clarification if needed. |

## Remaining technical uncertainty

Actual Uliya invocation and NAS installation, expert attribution quality, source-to-observation association, single-file scope enforcement, path resolution and permission behavior, PDF/OCR support, model choice and data locality, calculation and financial-basis interpretation are unverified. The Skill's instructions do not prove runtime read-only behavior or that configured models process data locally. All seven NAS tests in `MILESTONE-1.md` remain **blocked / not run**; local fixture checks and a static checker are not end-to-end tests.

## Before the entrant submits

1. Independently obtain the official creation tool from the starter kit and rerun its checker on the exact Skill, or obtain organizer confirmation that the hash-matched checker satisfies the official-tool requirement. If the result or Skill changes, rebuild and re-audit both artifacts.
2. Review the proposal and Skill for accuracy, rights, authorship, eligibility, team consent, license and tool-use disclosures; resolve any questions with the organizer. Review the linked SkillHub terms and privacy policy personally.
3. Select the appropriate track and complete any current Devpost form fields yourself. Upload the PDF and ZIP as **separate** artifacts; do not include this audit or fixtures in the ZIP.
4. Verify the current deadline and instructions before submission. On the checked official page, the Round 1 deadline is **15 November 2026 at 11:45 p.m. Pacific Time** (16 November, 8:45 a.m. WAT). No Round 1 video is required by the checked pages.
