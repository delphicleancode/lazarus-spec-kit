---
description: "Regras de desenvolvimento FreePascal/Lazarus (Object Pascal) — Convenções, SOLID, Clean Code"
globs: ["**/*.pas", "**/*.pp", "**/*.lpr", "**/*.lpk", "**/*.lfm"]
alwaysApply: false
---

# Regras Lazarus — Antigravity / Gemini

Consulte `AGENTS.md` na raiz do projeto para a referência completa.

## Ativacao de Skill de Migracao

Quando o pedido mencionar migracao Delphi -> Lazarus, conversao IntraWeb, FireDAC -> Zeos,
DFM -> LFM, ajuste de `System.*`/`VCL.*`, ou comando `RETOMAR` de migracao:

- Ativar skill completa: `.gemini/skills/delphi-lazarus-migration/SKILL.md`
- Se o usuario pedir versao enxuta/checklist rapido: `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md`
- Respeitar regra de equivalencia funcional (sem refatorar regra de negocio)

### Prioridade Automatica da Versao Quick

Se o prompt contiver termos como `curto`, `resumo`, `rapido`, `checklist`,
`versao curta` ou `objetivo`, priorizar:

- `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md`

Se o usuario pedir detalhamento completo, voltar para:

- `.gemini/skills/delphi-lazarus-migration/SKILL.md`

## Resumo de Convenções

- **PascalCase** para identificadores, palavras reservadas em minúsculas
- Prefixos obrigatórios: `T` (classes), `I` (interfaces), `E` (exceptions), `F` (campos), `A` (parâmetros), `L` (variáveis locais)
- Units: `NomeProjeto.Camada.Dominio.Funcionalidade.pas`
- Componentes em forms: prefixo de 3 letras (`btn`, `edt`, `lbl`, `cmb`, etc.)

## Princípios SOLID

1. **SRP** — Uma classe = uma responsabilidade. Separar Validator, Repository, Service
2. **OCP** — Extensão via interfaces, não modificação de classes existentes
3. **LSP** — Subtipos substituíveis pelo tipo base
4. **ISP** — Interfaces pequenas e coesas (IReadable, IWritable separados)
5. **DIP** — Depender de interfaces, constructor injection para dependências

## Clean Code

- Métodos ≤ 20 linhas (ideal: 5-10)
- Nomes auto-descritivos (verbos para métodos, substantivos para properties)
- Guard clauses em vez de nesting profundo
- Constantes nomeadas em vez de números mágicos
- Try/except focado com exceptions específicas
- Try/finally para gerenciamento de memória

## Padrões Lazarus vs Delphi (Evite Delphi-ismos)

- **UI:** Sempre `.lfm`, rejeite `.dfm`.
- **Interfaces:** Inclua GUID (`['{GUID}']`) e evite usar `override` ao implementar métodos da interface na classe.
- **Generics:** Use `specialize TFPGList<T>` (unit `fgl`) preferencialmente.
- **Bibliotecas:** Prefira `FpJSON` a `System.JSON`. Use `DefaultFormatSettings` (sysutils) no lugar de opções Windows-only.

## Proibições

- ❌ `with` statement
- ❌ Variáveis globais
- ❌ Lógica de negócio em event handlers de forms
- ❌ Catch genérico (`except on E: Exception`)
- ❌ God classes / God units
- ❌ Strings hardcoded
- ❌ Ignorar `Free` de objetos temporários

## Arquitetura em Camadas

```
Domain → Entidades, Value Objects, Interfaces
Application → Services, Use Cases, DTOs
Infrastructure → Repositories (SQLdb/Zeos), APIs
Presentation → Forms LCL, ViewModels
```

Regra: `Presentation → Application → Domain ← Infrastructure`

## Frameworks

Consulte skills específicas para cada framework:

- **Horse:** `.gemini/skills/horse-framework/SKILL.md` — APIs REST minimalistas (Express-like)
- **Intraweb:** `.gemini/skills/intraweb-framework/SKILL.md` — Desenvolvimento web (LCL for the Web) stateful
- **ACBr:** `.gemini/skills/acbr-components/SKILL.md` — Bibliotecas Fiscais/Automação Comercial
- **Firebird Database:** `.gemini/skills/firebird-database/SKILL.md` — Conexão, PSQL, generators, transactions
- **PostgreSQL Database:** `.gemini/skills/postgresql-database/SKILL.md` — Conexão, PL/pgSQL, UPSERT, JSONB, full-text search
- **MySQL Database:** `.gemini/skills/mysql-database/SKILL.md` — Conexão, AUTO_INCREMENT, UPSERT, JSON, stored procedures
- **Threading:** `.gemini/skills/threading/SKILL.md` — TThread, Synchronize/Queue, thread-safety
- **TDD (FPCUnit):** `.gemini/skills/tdd-fpcunit/SKILL.md` — Test-Driven Development, Fakes, FPCUnit
- **Clean Code:** `.gemini/skills/clean-code/SKILL.md` — Padrões pragmáticos de código limpo
- **Code Review:** `.gemini/skills/code-review/SKILL.md` — Checklist de revisão de código
- **Delphi -> Lazarus Migration:** `.gemini/skills/delphi-lazarus-migration/SKILL.md` — Migração com equivalência funcional e checkpoint
- **Delphi -> Lazarus Migration (Quick):** `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md` — Versão curta baseada em checklist
