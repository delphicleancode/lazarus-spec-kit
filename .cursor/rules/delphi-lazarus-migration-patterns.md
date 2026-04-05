---
description: "Ativacao e guardrails para migracao Delphi/IntraWeb -> Lazarus com equivalencia funcional"
globs: ["**/*.pas", "**/*.lfm", "**/*.lpr", "**/*.lpi", "**/*.dpr", "**/*.dproj", "**/*.dfm"]
alwaysApply: false
---

# Delphi -> Lazarus Migration - Cursor Rules

Aplique estas regras quando o pedido envolver migracao Delphi para Lazarus.

## Trigger de Ativacao

Ativar quando houver termos como:
- migrar Delphi para Lazarus
- migracao IntraWeb
- FireDAC para Zeos
- DFM para LFM
- System.* ou VCL.* para Lazarus/FPC
- RETOMAR migracao

## Modo de Skill

- Modo completo: usar `.gemini/skills/delphi-lazarus-migration/SKILL.md`
- Modo curto/checklist: usar `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md`

### Prioridade Automatica da Versao Quick

Se o prompt contiver termos como `curto`, `resumo`, `rapido`, `checklist`,
`versao curta` ou `objetivo`, priorizar:

- `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md`

Se o usuario pedir detalhamento completo, usar:

- `.gemini/skills/delphi-lazarus-migration/SKILL.md`

## Guardrails Obrigatorios

1. Nao refatorar logica de negocio.
2. Nao corrigir SQL/tabelas/colunas.
3. Nao alterar funcionalidades.
4. Nao criar configuracao .ini de banco.
5. Atualizar migration_log.md a cada arquivo concluido.

## Sequencia Minima

1. Converter estrutura do projeto (.dpr/.dproj/.dfm).
2. Converter units Delphi para FPC/Lazarus.
3. Converter FireDAC para Zeos no DataModule.
4. Compilar e registrar status.
