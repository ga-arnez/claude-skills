# claude-skills

Marketplace pessoal de plugins/skills para Claude (app) e Claude Code.

## Instalação

- **App Claude**: Configurações → Plugins → Adicionar marketplace → `ga-arnez/claude-skills` → instalar `gabriel-skills`.
  O Claude Code recebe automaticamente via sincronização da conta.
- **Só Claude Code**: `/plugin marketplace add ga-arnez/claude-skills` e `/plugin install gabriel-skills@ga-arnez-skills`.

## Atualizar

Edite em `plugins/gabriel-skills/skills/<skill>/SKILL.md`, aumente `version` em
`plugins/gabriel-skills/.claude-plugin/plugin.json` e faça `git push`.
Skills novas entram aqui — nunca direto em `~/.claude/skills/`.
