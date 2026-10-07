# oh-skills

Claude Code skills by Oliver Wehrens.

## Install

```
/plugin marketplace add oliverwehrens/oh-skills
/plugin install oh-skills@oh-skills
```

Skills are then available as `/oh-skills:<skill-name>` and trigger automatically from their descriptions.

## Skills

| Skill | Description |
|-------|-------------|
| `hello-world` | Example skill to verify the install |

## Adding a skill

Create `skills/<skill-name>/SKILL.md` with `name` and `description` frontmatter. Bump `version` in `.claude-plugin/plugin.json` so users get the update.

## License

MIT
