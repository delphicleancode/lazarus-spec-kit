# Claude Skills

Este diretório mantém as skills do projeto no formato adotado para o Claude.

## Padrão do repositório

- Cada skill fica em `.claude/skills/<skill-slug>/SKILL.md`
- O mesmo `<skill-slug>` deve existir em `.gemini/skills/<skill-slug>/SKILL.md`
- O conteúdo das skills de Claude e Gemini deve permanecer sincronizado

## Ativação de Skills de Migração

Para pedidos com termos como "migrar Delphi para Lazarus", "IntraWeb", "FireDAC para Zeos",
"DFM para LFM" ou "RETOMAR" em contexto de migração:

- Use `.claude/skills/delphi-lazarus-migration/SKILL.md` para fluxo completo
- Use `.claude/skills/delphi-lazarus-migration-quick/SKILL.md` para checklist curto

Guardrails obrigatórios:
- sem refatorar regra de negócio
- sem corrigir SQL/tabelas/colunas
- sem alterar funcionalidade
- atualizar `migration_log.md` por arquivo migrado

### Prioridade Automática da Versão Quick

Se o prompt contiver termos como "curto", "resumo", "rápido", "checklist",
"versão curta" ou "objetivo", priorize:

- `.claude/skills/delphi-lazarus-migration-quick/SKILL.md`

Se o usuário pedir detalhamento completo, use:

- `.claude/skills/delphi-lazarus-migration/SKILL.md`

## Empacotamento

Para subir uma skill no Claude, compacte a pasta da skill inteira, mantendo a pasta da skill como raiz do arquivo compactado.

Exemplo:

```text
firebird-database.zip
└── firebird-database/
    └── SKILL.md
```

## Observação

Embora parte da documentação pública do Claude cite `Skill.md`, a especificação aberta e os exemplos oficiais do Anthropic usam `SKILL.md`. O projeto adota `SKILL.md` para manter consistência entre Claude, Gemini e o ecossistema Agent Skills.