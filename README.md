# claude-plugins

Duncan's Claude Code plugin marketplace. It has one plugin, `duncan`, which
holds Duncan's personal skills.

## Install

```
claude plugin marketplace add DuncanDHall/claude-plugins
claude plugin install duncan@duncan-plugins
```

Toby installs it in every sandbox (`toby-apply`) and for `agents` outside
sandboxes (`setup-agents.sh`), from the `toby-build` repo.

## Add a skill

1. Make `plugins/duncan/skills/NAME/SKILL.md`:

   ```markdown
   ---
   name: NAME
   description: What the skill does. Use when ...
   ---
   The steps for Claude.
   ```

   Claude reads `description` to decide when to use the skill.
2. Increase `version` in `plugins/duncan/.claude-plugin/plugin.json`.
3. Run `claude plugin validate .`, then commit and push.
4. To get the change on a machine: `claude plugin marketplace update duncan-plugins`,
   then `claude plugin update duncan@duncan-plugins`, then restart Claude.

## Layout

| Path | What it is |
|------|------------|
| `.claude-plugin/marketplace.json` | The marketplace, `duncan-plugins`: the list of plugins in this repo. |
| `plugins/duncan/.claude-plugin/plugin.json` | The `duncan` plugin's name and version. |
| `plugins/duncan/skills/NAME/SKILL.md` | One skill. |
