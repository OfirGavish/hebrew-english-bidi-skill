# Validation record - 2026-11-17

## Result and scope

**The synthetic Word and PowerPoint repair-and-repeat fixtures passed in
Cowork and an unpublished Copilot Studio agent, with independent native Office
verification.** This is not certification of every document, format, renderer,
or feature described by the skill.

| Check | Cowork | Copilot Studio |
| --- | --- | --- |
| Word package, exact text/whitespace, styling, links, lists, table/page geometry | Pass | Pass |
| Word native direction and physical alignment | 18/18 | 18/18 |
| Word terminal-punctuation regression cases | 7/7 | 7/7 |
| Word native rendered pages inspected | 2 | 2 |
| PowerPoint package, text/whitespace, styling, links, lists, table/shape geometry, masters | Pass | Pass |
| PowerPoint direction/alignment cases, including speaker notes | 17/17 | 17/17 |
| PowerPoint native slide images inspected | 4 | 4 |
| Content-bearing native notes page inspected | Slide 4 | Slide 4 |
| First and second delivered files within each final run | Byte-identical | Byte-identical |

The seven Word punctuation checks overlap the 18 paragraph cases; they are not
seven additional paragraphs. PowerPoint has 16 slide cases and one notes case.
Slide 1-3 notes contain no test text. Inputs are synthetic and contain no
customer documents.

## Download evidence

- [Word inputs, outputs, native page images, and reports](./test-evidence/word-regression-evidence.zip).
- [PowerPoint input, outputs, native slides/notes, reports, and structural verifier](./test-evidence/powerpoint-regression-evidence.zip).

**These are evidence archives, not skill-upload ZIPs.** Install
[hebrew-english-bidi.zip](./hebrew-english-bidi.zip) instead.
Human-facing evidence is deliberately outside the agent payload and the CAT
submission. The archives do not include private browser traces, account URLs,
credentials, or workplace documents.

Native images were exported with Microsoft Word/PowerPoint 16.0 and inspected
by the AI assistant; the PowerPoint slide review used a separate visual-review
agent. This is not an independent human Hebrew-language certification.

## Tested payloads and activation evidence

The baseline was the instruction payload in
[microsoft/cat-agent-skills#388](https://github.com/microsoft/cat-agent-skills/pull/388)
at `9c20ad76f380885e2923761da884933a502b469b`. All six instruction/reference
files were matched by Git blob hash before the original tests.

Only the Office reference and validation corpus changed during the corrections.
The local final instruction files are mirrored in the CAT submission. The
payload also includes `LICENSE.txt`, preserving the MIT notice and avoiding
PowerPoint's rejection of the earlier extensionless license filename.

| Recorded artifact | SHA-256 |
| --- | --- |
| Word-tested skill ZIP | `7de3c5314315e94aa8d59b75d9123a3cc2de75dff3d54f4e24f66f2e7f9dca0e` |
| Final Office-tested skill ZIP | `04e59d71b8d5c7f7ecdc8e08e3ed8ae80096605e1fd24f3f81a2200e3d9eb15d` |
| Final Office reference bytes | `d30037b20807fa670f6eef46faadfb4788e4e0f7943efb3e082154364ce8bb86` |
| Final validation reference bytes | `8a67870abd362b7a1fb2e0403b6b3ea00ab4eb63eb7c16fa606a7c71901315a5` |

The later PowerPoint refinement leaves the tested Word guidance section
unchanged. Its LF-normalized SHA-256, before the PowerPoint section heading, is
`806e21c564ab397570d721cbf473ca3597fbcd0277c19520053afef497045f9c`.
ZIP container hashes describe captured artifacts, not every future rebuild.

Cowork's picker explicitly selected the imported custom skill; its activity
log showed custom-skill review and reference reads. Copilot Studio imported
the same candidate into the dedicated test draft and used fresh preview
conversations. Both final runs reported full Office/validation reference hashes
that matched the uploaded candidate's raw bytes. These reported fingerprints
and visible selection are evidence of the custom workflow, **not an
independently captured low-level tool-execution trace**. Built-in document
preprocessing was also used where required by the host; it was not treated as
a substitute for the custom bidi instructions.

## Defects found and corrected

1. **Word physical alignment:** raw `w:jc="right"` in an RTL paragraph produced
   physical left alignment in native Word. The guidance now prefers logical
   start alignment where supported and requires effective/native verification.
2. **Word neutral punctuation:** a final period after an LTR technical token
   needs the surrounding Hebrew sentence context, even in a punctuation-only
   run. Text, whitespace, emphasis, and token-internal punctuation stay intact.
3. **PowerPoint inline context:** paragraph RTL alone left boundary spaces,
   colons, parentheses, and terminal periods incorrect. The reference now
   requires logical mixed-run splitting where needed, matching Hebrew/English
   language context plus native run direction, and sentence/token ownership.
4. **Notes test visibility:** the first generated fixture had zero-size notes
   placeholders. Their text was present but could not be visually assessed.
   A separately generated fixture made notes visible *before* platform testing;
   the agents then preserved that input geometry. The hidden-note baseline is
   not counted as a visual pass.

Earlier failing results were retained rather than relabeled as passes. Local
diagnostic files established the causes, but only actual downloaded platform
outputs are counted in the table above.

## Rendering and capability limits

- **Cowork:** used its LibreOffice-based renderer. It disclosed minus-sign and
  `@mention` display artifacts that were absent in the independently inspected
  native Office outputs. Do not claim flawless cross-renderer behavior.
- **Copilot Studio:** generated editable files and honestly reported that no
  usable renderer was available. All native visual verification here was
  performed externally after downloading its output. Available sandbox tools
  varied by run; neither the skill nor a platform tag installs a renderer.
- **Copilot in PowerPoint:** the user confirmed successful custom-skill ZIP
  upload after packaging the license as `LICENSE.txt`. The separate in-app
  document-repair workflow was **not tested** by these Cowork/Studio runs.
- **Not covered:** other file formats; tracked changes, comments, inherited
  RTL styles, grouped objects, charts, SmartArt, encryption, or signatures.
  Their presence in the instructions describes conditional workflows, not
  completed acceptance tests.

Native reopening, schema checks, text comparison, and editability/shape checks
were performed. Copy/paste of technical tokens through each cloud host's live
editor was not independently tested.

## Reproduce the scoped tests

1. Install the exact skill payload and start a fresh task or preview. Explicitly
   select/request the custom skill, its Office reference, and its validation
   reference. Request their hashes without supplying the expected hashes.
2. Upload the corresponding synthetic input from the evidence archive. Specify
   author intent: Hebrew-base RTL, including the English-leading Hebrew case;
   English-base `Status:` and English-only controls LTR; preserve centering.
3. Request formatting-only repair with exact logical text, whitespace, styles,
   links, lists, geometry, and editability preserved. Forbid Unicode controls,
   translation, reversal, flattening, redesign, shared-master changes, and
   workplace searches. Include the header/footer or speaker notes as applicable.
4. Request a separate native output, then run the same procedure again. Require
   unchanged verification copies to be labeled honestly. Download both files
   inside an outer results archive to avoid an online preview rewriting them.
5. Validate package/schema integrity and preserved content, compare repeat
   files, and reopen/render the actual output in native Office. Inspect every
   changed page/slide and the content-bearing notes page. Do not grade rendering
   from XML or a claimed tool result alone.

The PowerPoint evidence includes a small Node structural verifier and fixed
baseline snapshots for both formats. Its reproduction instructions distinguish
structural results from visual review. It does not repair documents or replace
native rendering.

## Repository checks

- Four verification-harness tests passed, including negative controls,
  Word alignment-token and neutral-punctuation regressions, duplicate-direction
  detection, and protected-master change detection.
- `npm run check:submissions` passed all **161 submissions** in the CAT checkout.
- The configured package feed lacked the locked `mdast-util-to-hast@13.2.2`.
  A non-locking local restore through that same feed enabled the validator;
  tracked dependency manifests and lockfiles remained unchanged. This is not
  a claim that exact-lockfile `npm ci` was repaired, and no gallery/site
  dependency changes are part of the skill PR.
