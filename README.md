# qualcomm-skills

Sample Skills Hub repository for testing the Qualcomm IDE Skills Hub feature.

## Structure

```
skills.json                          ← catalog manifest (required at repo root)
skills/
  gpio-controller/
    SKILL.md                         ← documentation + frontmatter
  wifi-manager/
    SKILL.md
  uart-bridge/
    SKILL.md
```

## How it works

The IDE fetches `skills.json` from the repo root. Each entry points to a skill directory via `path` and its documentation via `docPath`. When a user installs a skill, all files matched by the `files` glob are downloaded to `~/.claude/skills/<name>/` (or `~/.codex/skills/<name>/`).

## Skills

| Name | Category | Agents | Platforms |
|---|---|---|---|
| `gpio-controller` | hardware | claude, codex | linux, qcs6490, rb3gen2 |
| `wifi-manager` | connectivity | claude | linux, ubuntu |
| `uart-bridge` | hardware | claude, codex | linux, qcs6490 |

## Using this repo in the IDE

Add this entry to `SKILL_REPOSITORIES` in `skills-hub.service.ts`:

```
qualcomm-developer/qualcomm-skills@main
```
