# markdownlint-skill

An Agent Skill for writing and fixing Markdown that passes markdownlint. It
covers headings, lists, line length, whitespace, and code blocks, then asks the
agent to run lint and fix the remaining errors. The target project's
configuration takes precedence.

The bundled baseline keeps markdownlint's default rules and sets an
80-character line limit, with tables and code blocks exempt from that limit.
The skill requires explicit approval before disabling rules or suppressing
errors.

## Layout

```text
.
├── SKILL.md             # discovery metadata, procedure, checklist, validation
├── .markdownlint.yaml   # baseline config, used when the project has none
└── README.md            # installation and usage
```

The [Agent Skills format](https://agentskills.io/specification) lets tools read
the name and description for discovery and load the full procedure when needed.
This skill keeps its instructions in one file and its lint baseline alongside
it.

## Install

Clone the repository into your tool's skills directory. For Codex and other
tools that read `~/.agents/skills/`:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/h0tc0d3/markdownlint-skill.git ~/.agents/skills/markdownlint-skill
```

Two things matter when copying this skill:

1. Name the containing directory `markdownlint-skill`, matching `name` in the
   frontmatter.
2. Keep `.markdownlint.yaml` beside `SKILL.md`. It is a hidden file, so copying
   only `*.md` leaves out the fallback configuration.

Installing the skill does not install the linter. The agent uses the project's
existing command when available. The fallback commands use Node.js, npm, and
[markdownlint-cli](https://github.com/igorshubovych/markdownlint-cli); `npx` may
download the package if it is not already installed.

## Where each tool looks

Put the skill inside one of these directories so its entry point lands at
`<skills dir>/markdownlint-skill/SKILL.md`. The tool names link to their skill
documentation.

| Tool | Project skills dir | Personal skills dir |
| --- | --- | --- |
| [Codex](https://developers.openai.com/codex/skills/) | `.agents/skills/` | `~/.agents/skills/` |
| [Claude Code](https://code.claude.com/docs/en/skills) | `.claude/skills/` | `~/.claude/skills/` |
| [OpenCode](https://opencode.ai/docs/skills/) | `.opencode/skills/`, `.agents/skills/`, `.claude/skills/` | `~/.config/opencode/skills/`, `~/.agents/skills/`, `~/.claude/skills/` |
| [Cursor](https://cursor.com/docs/skills) | `.cursor/skills/`, `.agents/skills/` | `~/.cursor/skills/`, `~/.agents/skills/` |
| [Gemini CLI](https://geminicli.com/docs/cli/skills/) | `.gemini/skills/`, `.agents/skills/` | `~/.gemini/skills/`, `~/.agents/skills/` |

Project paths are relative to the project root. `~` means your home directory;
on Windows, use the corresponding path under `%USERPROFILE%`.

## One copy, several tools

The `.agents/skills/` installation above serves Codex, OpenCode, Cursor, and
Gemini CLI. To share it with Claude Code, link the same directory:

```bash
mkdir -p "$HOME/.claude/skills"
ln -s "$HOME/.agents/skills/markdownlint-skill" \
  "$HOME/.claude/skills/markdownlint-skill"
```

Use an unused destination for the link. If it already contains a skill copy,
move that copy aside first. Copy the two files instead where symlinks are
unavailable. Re-copy them after updates, or update the shared directory when
using a link.

## Using it

Tools can match Markdown work against the skill's description. To request it
explicitly, name it in your prompt:

```text
Use the markdownlint-skill skill to update README.md and check the result.
```

```text
Use markdownlint-skill to fix the Markdown lint errors in docs/.
```

Once active, the agent finds the project's configuration, makes the requested
changes, runs the applicable linter, and resolves its diagnostics. It uses the
bundled baseline only when the target project has no Markdown lint
configuration. Automatic fixes are reviewed and followed by another lint run.

To check this repository yourself:

```bash
npx markdownlint-cli --config .markdownlint.yaml SKILL.md README.md
```

## Confirm it loaded

Ask your agent to list the skills it can see and look for `markdownlint-skill`.
Then give it a small Markdown task. Check that it reads `SKILL.md`, finds the
applicable configuration, and runs lint before reporting success. If the
linter cannot run, it should state that validation was unavailable.

## No skill support?

Copy the body of `SKILL.md`, without the YAML frontmatter, into the rules file
your tool reads, such as `AGENTS.md` or `CLAUDE.md`. Keep the baseline config
accessible and update its path in the copied instructions. Always-loaded rules
use context on every turn instead of loading only for Markdown work.

For a single task, attach `SKILL.md` and `.markdownlint.yaml` to the prompt and
ask the agent to apply them.
