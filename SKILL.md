---
name: markdownlint-skill
description: >-
  Write, edit, and review Markdown that passes markdownlint. Use when creating
  or changing .md files, READMEs, guides, or documentation, or when fixing
  Markdown lint errors. Follow the target project's configuration first.
---

# Markdownlint

All `.md` files must pass markdownlint. Project configuration takes precedence
over this skill's baseline. Preserve the document's meaning while fixing format.

## When to use

Apply when creating, editing, or reviewing Markdown files, including other
`SKILL.md` files. Validate every Markdown file you create or change. For a
repository-wide lint request, check all Markdown files covered by that task.

## Procedure

1. Find the target project's Markdown lint command, configuration, and ignore
   rules. Check repository instructions and existing scripts before choosing a
   runner. Use any directory-specific configuration supported by that runner.
2. Follow the project's configured rules. If it has no Markdown lint
   configuration, use the bundled [.markdownlint.yaml](.markdownlint.yaml):
   markdownlint defaults with 80-character lines, excluding tables and code
   blocks from the line-length check. Resolve this path from the skill's
   directory, which may differ from the target project's directory.
3. Write or edit using the checklist below. Keep changes within the task's scope.
4. Run the project's lint command on the affected Markdown files. If no command
   exists, use the commands under Validation with the applicable configuration.
5. Fix reported issues, review any automatic changes, and rerun lint until it
   passes. If the runner cannot execute, report that validation was unavailable
   and why; do not claim the files passed.

Do not disable rules, add suppression comments, weaken configuration, or expand
ignore patterns to bypass errors without explicit user approval. Existing
project settings remain authoritative.

## Formatting checklist

### Headings

- Use ATX headings: `#`, `##`, `###`. Avoid Setext underlines (MD003).
- Increase heading levels sequentially without skipping levels (MD001).
- Use one H1 per file, subject to the project's frontmatter title rules (MD025).
- Put one space after the heading marker (MD018, MD019).
- Leave a blank line before and after headings (MD022).

### Lists

- Prefer `-` for unordered lists, or the project's configured marker such as
  `*`. Keep the marker consistent within a file (MD004).
- Use one space after list markers unless configured otherwise (MD030).
- Indent nested unordered lists by two spaces unless configured otherwise
  (MD007). Align continuation text with the item text.
- Leave a blank line before and after list blocks (MD032).

### Lines and whitespace

- Wrap prose at the configured length, normally 80 characters (MD013).
  The bundled configuration exempts tables and code blocks. Long URLs can
  qualify for markdownlint's unbreakable-line exception; links do not exempt
  surrounding prose. Follow the project's settings if they differ.
- Remove trailing whitespace (MD009). Preserve intentional hard breaks only
  when the project's rules allow them.
- Avoid consecutive blank lines (MD012).
- Leave a blank line before and after fenced code blocks (MD031).
- End each file with exactly one newline (MD047, MD012).

These checks cover common errors; all other enabled markdownlint rules still
apply. Give fenced code blocks an appropriate language, such as `bash`, `yaml`,
or `text`, and address any additional diagnostics from the runner.

## Validation

Run from the target project's root so its configuration and ignore rules apply.
Prefer the project's existing runner and pinned dependency. Otherwise, with
Node.js and npm available, check a file using:

```bash
npx markdownlint-cli "path/to/file.md"
```

For fixable issues, apply fixes, review the changes, then check again:

```bash
npx markdownlint-cli --fix "path/to/file.md"
npx markdownlint-cli "path/to/file.md"
```

When no project configuration exists, explicitly select the bundled baseline.
Replace the skill path below with this skill's actual installed directory:

```bash
SKILL_DIR="/absolute/path/to/markdownlint-skill"
npx markdownlint-cli --config "$SKILL_DIR/.markdownlint.yaml" "path/to/file.md"
```

Use the same configuration when fixing and rechecking. For multiple files, pass
their paths together or use the project's established glob patterns. Automatic
fixes do not resolve every issue; a successful final check is required.
