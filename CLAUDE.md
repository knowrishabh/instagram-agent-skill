# CLAUDE.md

Instagram agent skill pack: 13 Claude skills (`skills/ig-*/SKILL.md`) plus six
stdlib-only Python tools. Distributed as a Claude Code plugin via
`.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`.

## Layout

- `skills/ig-*/SKILL.md` - one skill per folder; frontmatter `name` must match the folder.
- Python tools (no dependencies, no network, Python 3 stdlib only):
  - `skills/ig-reel/hookscore.py`, `skills/ig-reel/beats.py` (data: `hooks.json`)
  - `skills/ig-caption/caption.py`
  - `skills/ig-human/humanize.py`, `skills/ig-human/detect.py` (data: `slop.json`)
  - `skills/ig-viral/swipe.py`
  - `skills/ig-profile/rubric.json` is data only.
- `templates/voice.md` - user voice template; skills read `~/.claude/instagram/voice.md`.
- `.claude/skills` is a symlink to `skills/`, so every `/ig-*` skill is available
  in Claude Code sessions opened in this repo.

## Checks (run before committing)

```bash
python3 -m py_compile skills/*/*.py
for f in skills/*/*.py; do python3 "$f" --help >/dev/null || echo "FAIL $f"; done
python3 -c "import json,glob;[json.load(open(f)) for f in glob.glob('skills/*/*.json')+glob.glob('.claude-plugin/*.json')]"
```

There is no test suite; smoke-test a changed tool on a small text file.

## Conventions

- Keep the tools dependency-free: no pip packages, no network calls.
- Tools resolve their data files relative to their own folder (`HERE = os.path.dirname(...)`).
- Prose in skills and README avoids em dashes and the stock phrases in
  `skills/ig-human/slop.json`; run `detect.py` on new copy.
- Bump `version` in `.claude-plugin/plugin.json` on releases; keep the skill
  count in README, plugin.json and marketplace.json consistent.
