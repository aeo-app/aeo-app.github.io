---
name: markdown-to-docx
description: "Convert a Markdown document into a DOCX file under the repository's output directory. Use when exporting Markdown content, schedules, reports, or the AEO Intel content library to Word format."
argument-hint: "[path/to/input.md] [output-name.docx]"
user-invocable: true
---

# Markdown to DOCX

Convert a Markdown source file into a Microsoft Word document while keeping generated artifacts in `output/`.

## Requirements

- `pandoc` must be installed and available on `PATH`.
- The input must be a Markdown file in the workspace.

## Procedure

1. Resolve the input path from the user's argument. If no input is supplied, use the repository's primary Markdown content file, `projects/AEO-Intel_Full_Schedule_and_Content_Library.md`. The repo also ships `projects/APAC-Relocation_30-Day_AEO-GEO_Schedule_and_Content_Library.md` — a valid alternative input when the user names it.
2. Resolve the output name from the user's argument. If no name is supplied, use the input filename with its extension changed to `.docx`.
3. Create the output directory if it does not exist:

   ```sh
   mkdir -p output
   ```

4. Convert the document with Pandoc:

   ```sh
   pandoc "<input.md>" --from=gfm --to=docx --output="output/<output-name>.docx"
   ```

5. Confirm the output exists and is non-empty:

   ```sh
   test -s "output/<output-name>.docx"
   ```

6. Report the relative output path and note that files under `output/` are ignored by Git.

## Guardrails

- Keep the source Markdown unchanged.
- Never write generated DOCX files outside `output/` unless the user explicitly asks for another destination.
- Quote paths so filenames containing spaces are handled correctly.
- If `pandoc` is unavailable, report the prerequisite clearly and do not create a partial artifact.
