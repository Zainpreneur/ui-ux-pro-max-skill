# ui-ux-pro-max

UI/UX design intelligence skill for OpenCode (and any other agent that reads `SKILL.md`).
It is a local, searchable design database — no network calls, no API keys, no
dependencies beyond Python 3.

Fork of [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
(MIT), repackaged so the scripts resolve their paths portably and so every
command works from OpenCode without a Claude Code plugin root.

## What is inside

| Domain | Contents |
| ------ | -------- |
| `styles` | 79 searchable styles (50 active) with keywords, performance and a11y risk |
| `product` | 192 product palettes + reasoning profiles |
| `color` | Semantic token sets (`--color-primary`, `--color-on-primary`, …) |
| `typography` | 74 font pairings, plus individual Google Fonts |
| `ux` | 119 UX guidelines with Do/Don't, code samples and severity |
| `icons` | 105 curated icons |
| `gsap` | 17 motion presets tied to motion tiers |
| `chart` | 25 chart types |
| `landing` | Landing page section patterns |
| `react` | React/Next.js performance rules |
| `web` | Native app interface guidelines |
| `stacks` | 22 implementation stacks (react, nextjs, vue, svelte, astro, nuxtjs, nuxt-ui, angular, laravel, swiftui, react-native, flutter, jetpack-compose, html-tailwind, shadcn, threejs, javafx, wpf, winui, avalonia, uno, uwp) |

Layout:

```text
.claude/skills/ui-ux-pro-max/
├── SKILL.md
├── data/          # 15 domain CSVs, 22 stack CSVs, reasoning rules
├── references/    # quick-reference.md (all rules), pro-rules.md (pre-delivery checklist)
└── scripts/       # search.py, design_system.py, core.py, validate_data.py, tests
```

## Install

### OpenCode — global (all projects)

```bash
git clone https://github.com/Zainpreneur/ui-ux-pro-max-skill /tmp/ui-ux-pro-max-skill
mkdir -p ~/.config/opencode/skills
cp -R /tmp/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max ~/.config/opencode/skills/
```

OpenCode picks it up automatically; skills are advertised by ID (`ui-ux-pro-max`)
and loaded on demand. No restart needed.

### OpenCode — one project only

```bash
git clone https://github.com/Zainpreneur/ui-ux-pro-max-skill /tmp/ui-ux-pro-max-skill
mkdir -p .opencode/skills
cp -R /tmp/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max .opencode/skills/
```

Project skills override a global skill with the same ID, so this is also how you
pin a project to a specific version.

### OpenCode — keep it in a shared folder

Point `opencode.jsonc` at the cloned repo instead of copying files. Array entries
combine across config files; relative paths resolve from the active working
directory.

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "skills": ["~/src/ui-ux-pro-max-skill/.claude/skills"]
}
```

Put that in `~/.config/opencode/opencode.jsonc` for every project, or in a
project's `.opencode/opencode.jsonc` for one.

### Claude Code

The repo already uses Claude's layout, so cloning it into `~/.claude` is enough:

```bash
git clone https://github.com/Zainpreneur/ui-ux-pro-max-skill ~/.claude/plugins/ui-ux-pro-max
```

Or copy `.claude/skills/ui-ux-pro-max` into `~/.claude/skills/`.

### Agent-agnostic

Any host that discovers `SKILL.md` directories can read it. Drop the
`ui-ux-pro-max` folder into that host's skills directory
(`~/.agents/skills/`, `.agents/skills/`, etc.).

## Use

The skill loads itself when a task is about UI: pages, components, design
systems, accessibility, responsive layout, typography, color, charts, motion.
You can also force it with `@ui-ux-pro-max` in a prompt, or the `skill` tool.

Once loaded, it drives a local search script. `<skill-dir>` is the directory
that contains `SKILL.md`:

```bash
# Full design system for a new page or product
python3 <skill-dir>/scripts/search.py "beauty spa wellness service" --design-system -p "Serenity Spa"

# One focused concern
python3 <skill-dir>/scripts/search.py "keyboard focus modal" --domain ux

# Implementation guidance for a detected stack
python3 <skill-dir>/scripts/search.py "suspense streaming bundle" --stack nextjs
```

Options:

| Flag | Purpose |
| ---- | ------- |
| `--design-system` | Pattern + style + colors + typography + effects + anti-patterns |
| `-p "Name"` | Project name for the design system header |
| `--persist` | Write `design-system/<slug>/MASTER.md` (pass `--output-dir` too) |
| `--page "dashboard"` | With `--persist`, add a page-level override file |
| `--variance`, `--motion`, `--density` | 1–10 dials that tune the design system |
| `-f ascii\|markdown`, `--json` | Output format |
| `-n <count>` | Result count for `--domain` searches |

Verify the bundled data any time:

```bash
python3 <skill-dir>/scripts/validate_data.py
# OK: validated 12 domain files, 22 stack files, and ui-reasoning.csv
```

## Requirements

- Python 3 (standard library only). `python3`, or `python`, or `py -3` on Windows.
- No packages to install.

## Differences from upstream

- `SKILL.md` invocations use `<skill-dir>/scripts/search.py` instead of
  `${CLAUDE_PLUGIN_ROOT}/.claude/skills/ui-ux-pro-max/scripts/search.py`, which
  does not exist outside Claude Code plugins.
- Only the `ui-ux-pro-max` skill is included. Upstream also ships `brand`,
  `design`, `design-system`, `banner-design` and a plugin marketplace manifest.
- Python `__pycache__` and pytest caches are gitignored.

## License

MIT. Original work copyright (c) 2024 Next Level Builder — see [LICENSE](LICENSE).
