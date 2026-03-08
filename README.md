# Claude Skills

A collection of Claude Code skills for product management, workflow automation, and AI-assisted development.

## Installation

### Via Claude Code Plugin Marketplace

```
/plugin marketplace add tflaim/claude-skills
```

### Manual Install

Clone into your Claude Code skills directory:

```bash
git clone https://github.com/tflaim/claude-skills.git ~/.claude/skills/tflaim-claude-skills
```

Or install a single skill by copying its folder into `~/.claude/skills/`.

## Skills

| Skill | Description |
|-------|-------------|
| *Coming soon* | Skills will be added here as they are published |

## Structure

```
skills/
  skill-name/
    SKILL.md           # Entrypoint (required)
    references/        # Supporting docs
    scripts/           # Executable utilities
```

Each skill is a self-contained directory with a `SKILL.md` entrypoint containing YAML frontmatter and markdown instructions.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT
