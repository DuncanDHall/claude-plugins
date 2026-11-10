# claude-plugins

Duncan's Claude Code plugin marketplace. Start with `README.md`.

## Conventions

- Each change to a skill increases `version` in `plugins/duncan/.claude-plugin/plugin.json`.
- Run `claude plugin validate .` before each commit.
- This repo is public. Never put tokens or internal Internet Archive information in it.

## Glossary

| Term | Meaning |
|------|---------|
| Marketplace | A list of plugins that Claude Code can install from. This repo's is `duncan-plugins`, in `.claude-plugin/marketplace.json`. |
| `duncan` | The plugin in this repo, in `plugins/duncan/`. Install id: `duncan@duncan-plugins`. |
| Skill | One folder under `plugins/duncan/skills/` with a `SKILL.md`. Its `description` tells Claude when to use it. |
| Toby | Duncan's always-on dev box that runs sandboxed Claude Code agents. Its `toby-build` repo installs this plugin. |
