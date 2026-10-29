---
name: markdownlint-skill
description: >-
  Write, edit, or review Markdown (.md, READMEs, guides, docs, SKILL.md) and fix
  markdownlint errors. Follow the target project's configuration first.
---

# Markdownlint

Preserve meaning and task scope. Validate every Markdown file created or changed;
for repository-wide lint requests, check all Markdown files in scope.

## Workflow

1. Read repository instructions; find its lint command, configuration (including
   directory overrides), and ignore rules. Prefer its runner and pinned version.
2. Follow project rules over the checklist below. If no configuration exists,
   use [.markdownlint.yaml](.markdownlint.yaml) beside this skill: defaults with
   80-character lines, exempting tables and code blocks. Resolve that path from
   the installed skill directory, not the target project.
3. Edit, lint from the project root, fix diagnostics, review automatic changes,
   and rerun until a final check passes. Keep configuration consistent between
   fixes and checks. If lint cannot run, report why; do not claim it passed.

Without explicit user approval, never bypass errors by disabling rules, adding
suppression comments, weakening configuration, or expanding ignores.

## Formatting checklist

Unless project rules differ:

- Use ATX headings with one space after `#`; do not skip levels. Use one H1,
  subject to frontmatter title rules.
- Use consistent `-` list markers and one space after markers. Indent nested
  unordered lists two spaces; align continuation text with item text.
- Separate headings, lists, and fenced code blocks from surrounding content
  with blank lines. Give code fences an appropriate language (`bash`, `text`).
- Wrap prose at 80 characters. The baseline exempts tables and code blocks;
  unbreakable URLs may qualify for an exception, but surrounding prose does not.
- Remove trailing whitespace except permitted hard breaks. Avoid consecutive
  blank lines and end each file with exactly one newline.

All enabled rules apply; fix remaining diagnostics from the runner.

## Fallback validation

If the project has no lint command, use Node.js/npm from the project root:

```bash
npx markdownlint-cli "path/to/file.md"
```

Pass multiple paths or established project globs as needed. Add `--fix` for
automatic fixes, then review and rerun without it. When no project configuration
exists, add `--config /absolute/path/to/markdownlint-skill/.markdownlint.yaml`
to both commands, using this skill's actual installed path.
