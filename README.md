# Hebrew-English Bidi Skill

An English-language agent skill for creating and repairing mixed Hebrew and
English text without reversing letters, translating content, or flattening
editable documents into images.

**English instructions work with Hebrew documents.** You can invoke the skill
in English or Hebrew. English-only paragraphs remain left-to-right.
The skill separates paragraph direction, inline text direction, and alignment.

## Coverage and honest limits

| Format | Workflow |
| --- | --- |
| Word `.docx` | Native paragraph/run bidi settings; preserve styles, links, lists, and revisions. |
| PowerPoint `.pptx` | DrawingML paragraph/run direction; preserve slide layout, editability, and notes. |
| HTML | Semantic `dir` attributes and isolated opposite-direction phrases. |
| Markdown | Viewer-aware markup; no claim of portable RTL formatting across renderers. |
| Plain text | Viewer settings first; approved Unicode isolates only when necessary. |
| Excel `.xlsx` | Per-cell reading order, independent of alignment and sheet direction. |
| CSV / TSV | Preserve data; offer XLSX/HTML when persistent formatting is required. |
| Outlook / email | Native draft or MIME/HTML-aware editing; client-specific verification. |
| PDF | Prefer repairing the source and re-exporting; OCR/direct editing are conditional. |
| OpenDocument | ODF-aware direction settings and native-application verification. |
| Legacy Office, RTF, images, other formats | Tool-dependent editing, approved conversion, or a clear limitation. |

This is an **instruction skill**, not a standalone converter, Office add-in,
or bundled editing program. It uses whatever tools the agent host actually
provides. It does not guarantee support for every file type, and cannot give
plain-text formats formatting metadata they do not possess.

## Install

The canonical payload is
[`.github/skills/hebrew-english-bidi/`](./.github/skills/hebrew-english-bidi/).
Copy the **whole folder**, including its `references` directory, not just
`SKILL.md`.

### Download or manually upload the skill ZIP

**[Download hebrew-english-bidi.zip](https://github.com/OfirGavish/hebrew-english-bidi-skill/raw/refs/heads/main/hebrew-english-bidi.zip)**

This upload-ready ZIP contains one top-level `hebrew-english-bidi` folder with
`SKILL.md`, all five reference files, and the MIT license as `LICENSE.txt`.
It does not contain the repository's `.github` wrapper or catalog metadata.

For AI tools that support importing custom skills as ZIP files:

1. Download the skill ZIP using the link above. Do **not** use GitHub's
   **Code > Download ZIP**, which downloads the whole repository instead.
2. Open the tool's custom-skill management area and choose its upload or import
   action. The location and labels depend on the host.
3. Select `hebrew-english-bidi.zip` without extracting it, then follow the
   host's steps to enable the skill.

Use the host's **skill importer**, not an ordinary chat attachment, unless its
documentation explicitly describes attaching a ZIP as a skill-installation
method. ZIP import is not available in every AI tool, and installing the skill
does not add file-editing tools the host does not already provide.

If the host requires a folder instead, extract the ZIP and copy the complete
`hebrew-english-bidi` folder into its documented skills directory. Keep
`SKILL.md` directly inside that folder, with `references` alongside it.

### Copilot in PowerPoint

1. Open Copilot in PowerPoint and select **+ > Choose skills > Manage skills**.
2. Select **Add skill**, then the upload option, and upload the downloaded
   `hebrew-english-bidi.zip` without extracting it.
3. If needed, refresh **Manage skills**, then select the skill to use it.

PowerPoint rejected the earlier ZIP containing an extensionless `LICENSE` file
with the message **"This skill includes a file that can't be checked, so it
can't be used."** Changing only that packaged filename to `LICENSE.txt`
resolved the reported upload issue; the skill and license text are unchanged.
If you encounter this error with an older copy, download the latest ZIP above.

All files in the ZIP now use `.md` or `.txt` extensions, both listed in
[PowerPoint's supported skill file types](https://support.microsoft.com/en-us/powerpoint/copilot/copilot-in-powerpoint-skills#known-limitations).
Use these personal skill-upload instructions, not the separate organization-wide
admin deployment flow, which requires a different package.

### VS Code / GitHub Copilot

Clone this repository and work in it for workspace-local discovery, or copy the
skill into another project's `.github/skills/` directory.

For personal use across projects with a host that supports
`~/.copilot/skills/`, choose either installation method below.

**From the downloaded ZIP** (adjust `$zip` if you saved it elsewhere):

```powershell
$zip = Join-Path $HOME 'Downloads\hebrew-english-bidi.zip'
$skills = Join-Path $HOME '.copilot\skills'
$destination = Join-Path $skills 'hebrew-english-bidi'
if (Test-Path -LiteralPath $destination) {
    throw "Skill already exists at $destination; review before replacing it."
}
New-Item -ItemType Directory -Path $skills -Force | Out-Null
Expand-Archive -LiteralPath $zip -DestinationPath $skills
```

The ZIP already contains the `hebrew-english-bidi` folder, so extract into
`skills`, not into an additional `hebrew-english-bidi` subfolder.

**From Git**:

```powershell
git clone https://github.com/OfirGavish/hebrew-english-bidi-skill.git
$source = Join-Path $PWD 'hebrew-english-bidi-skill\.github\skills\hebrew-english-bidi'
$skills = Join-Path $HOME '.copilot\skills'
$destination = Join-Path $skills 'hebrew-english-bidi'
if (Test-Path -LiteralPath $destination) {
    throw "Skill already exists at $destination; review before replacing it."
}
New-Item -ItemType Directory -Path $skills -Force | Out-Null
Copy-Item -LiteralPath $source -Destination $destination -Recurse
```

For other Agent Skills-compatible hosts, use the host's documented skill
directory or import flow. Common conventions include `.agents/skills/` and
`.claude/skills/`. Installation does not install document-editing dependencies.

### Microsoft CAT Agent Skills

CAT contributions use an **unpacked** `submissions/hebrew-english-bidi/` folder
containing this skill payload and a human-facing `metadata.json` sidecar.
The [catalog metadata](./metadata.json) is kept outside the agent payload.

The intended gallery targets are **Cowork** and **Copilot Studio**, subject to
their actual file-editing and rendering capabilities. A platform tag is not a
claim of end-to-end testing in that host. If tools are unavailable, the skill
must say so and provide guidance rather than claim to save or verify a file.

After maintainers merge and deploy a submission, follow the gallery's current
platform-specific installation guidance:
[CAT Agent Skills](https://microsoft.github.io/cat-agent-skills/).
Publishing this repository does not itself publish a gallery entry.

## Use

Examples:

> Fix the Hebrew-English direction in this PowerPoint. Keep all text editable
> and preserve the design. Check all slides, table cells, and speaker notes.

> Fix mixed RTL/LTR text in this Word document. Preserve tracked changes,
> hyperlinks, list numbering, and English-only paragraphs.

> Fix the mixed Hebrew-English text in this Markdown for GitHub. Explain any
> viewer limitations instead of promising universal rendering.

> תתקן את הכיווניות של העברית והאנגלית במסמך בלי לשנות את העיצוב.

The default is to inspect all in-scope content and repair only affected text.
The original is preserved, and outputs use a separate name unless overwriting
is explicitly requested. Files are not uploaded to conversion services or
messages sent without permission.

## What changed from the original

The source was a short, Hebrew-only PowerPoint skill whose final preservation
instruction was incomplete. This version supplies a complete workflow,
English and Hebrew discovery phrases, format-specific guidance, explicit
preservation rules, and a verification checklist.

It distinguishes an English **skill/prompt** from an English **document**:
writing the instructions in English does not prevent Hebrew handling, and
handling Hebrew does not mean forcing English-only text into RTL.

## Validation

The skill includes a
[mixed-language corpus and regression checklist](./.github/skills/hebrew-english-bidi/references/validation.md).
These are acceptance criteria, not a claim that every renderer or host has
been tested. For a real document, verify package/syntax integrity, exact
logical text, preserved semantics, actual rendering, and second-pass
idempotency. If rendering is unavailable, report it explicitly.

Contributors should also check frontmatter, folder/name agreement, relative
reference links, absence of unintended invisible bidi controls, and CAT
metadata with the
[upstream submission validator](https://github.com/microsoft/cat-agent-skills/blob/main/CONTRIBUTING.md).

## Contribute

Open an issue or pull request with the format, application/version, logical
input, expected direction, and observed behavior. Use synthetic examples;
do not post private documents, credentials, or customer data.
Update the matching format reference and regression cases together.
Do not add a new format as "supported" without documenting its tools and limits.
When changing the skill payload, rebuild `hebrew-english-bidi.zip` to keep it
in sync, preserving the top-level skill folder and including the repository's
MIT `LICENSE` as `LICENSE.txt` inside that folder for PowerPoint compatibility.

## License

[MIT](./LICENSE). Copyright (c) 2026 Ofir Gavish.
