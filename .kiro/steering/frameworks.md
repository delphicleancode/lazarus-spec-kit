# Frameworks — FreePascal/Lazarus Spec-Kit

## Frameworks Suportados

Este spec-kit oferece suporte a frameworks robustos e consolidados no ecossistema FreePascal e Lazarus.

### Horse (REST API Minimalista)

- **Quando usar:** APIs REST, microsserviços, prototipagem rápida
- **Estilo:** Minimalista, inspirado no Express.js
- **Características:** Middleware chain, baixo acoplamento, rápido de configurar
- **Instalação:** `boss install horse`
- **Skills:** `.gemini/skills/horse-framework/SKILL.md`
- **Rules:** `.cursor/rules/horse-patterns.md`

### Projeto ACBr (Automação Comercial e Fiscal)

- **Quando usar:** Emissão de documentos fiscais (NF-e, NFC-e, CT-e, SAT), TEF, Boletos e acesso a hardware.
- **Arquitetura:** Não agrupe componentes visuais diretamente nos formulários. Crie Serviços/Adapters (`INFeService`) que isolem o componente `TACBrNFe`.
- **Skills:** `.gemini/skills/acbr-components/SKILL.md`
- **Rules:** `.cursor/rules/acbr-patterns.md`

### IntraWeb Framework

- **Quando usar:** Migração de ERPs desktop para Web com paradigma stateful baseado no servidor (Server-side rendered (AJAX/Postbacks)).
- **Atenção:** Evite o uso de variáveis globais (que vazam state cross-session). Direcione o estado transiente sempre em instâncias do `UserSession`.
- **Skills:** `.gemini/skills/intraweb-framework/SKILL.md`
- **Rules:** `.cursor/rules/intraweb-patterns.md`

### Delphi -> Lazarus Migration

- **Quando usar:** Port de projetos Delphi (especialmente IntraWeb) para Lazarus/FreePascal sem alterar regra de negocio.
- **Triggers:** migrar Delphi para Lazarus, FireDAC -> Zeos, DFM -> LFM, ajuste de `System.*`/`VCL.*`, comando `RETOMAR`.
- **Skill completa:** `.gemini/skills/delphi-lazarus-migration/SKILL.md`
- **Skill curta (checklist):** `.gemini/skills/delphi-lazarus-migration-quick/SKILL.md`
- **Rule Cursor:** `.cursor/rules/delphi-lazarus-migration-patterns.md`

### Firebird Database

- **Quando usar:** Banco principal do ecossistema FreePascal/Lazarus, com PSQL e transações ACID.
- **Acesso:** Via SQLdb/Zeos (driver `FB`).
- **Características:** Generators/Sequences, Domains, Triggers.
- **Regras críticas:** Dialect 3 SEMPRE, CharacterSet UTF8, manipulação via SQLdb.
- **Skills:** `.gemini/skills/firebird-database/SKILL.md`
- **Rules:** `.cursor/rules/firebird-patterns.md`

### PostgreSQL Database

- **Quando usar:** Projetos cloud-native ou microsserviços que usufruam de JSONB, Full-Text Search e CTEs.
- **Acesso:** Via SQLdb/Zeos (driver `PG`) — client library `libpq.dll`.
- **Características:** IDENTITY, JSONB indexável.
- **Skills:** `.gemini/skills/postgresql-database/SKILL.md`
- **Rules:** `.cursor/rules/postgresql-patterns.md`

### MySQL / MariaDB Database

- **Acesso:** Via SQLdb/Zeos (driver `MySQL`).
- **Características:** AUTO_INCREMENT, `LAST_INSERT_ID()`, JSON nativo.
- **Regras práticas:** `utf8mb4` SEMPRE (nunca `utf8`), `InnoDB` SEMPRE.
- **Skills:** `.gemini/skills/mysql-database/SKILL.md`
- **Rules:** `.cursor/rules/mysql-patterns.md`

### Threading & Multi-Threading

- **Quando usar:** Operações demoradas que bloqueiam a UI LCL.
- **Abordagens:** Derivação direta de `TThread` (herança do método `Execute`).
- **Regra de Ouro:** NUNCA acessar LCL de thread secundária cega → use `TThread.Synchronize` (bloqueante) ou `TThread.Queue` (não-bloqueante).
- **Thread-Safety:** `TCriticalSection` (sempre Enter antes e Leave do finally).
- **Skills:** `.gemini/skills/threading/SKILL.md`
- **Rules:** `.cursor/rules/threading-patterns.md`

## Decisão de Framework

```
Preciso de Banco de Dados?
├── Corporativo robusto com PSQL, transactions → Firebird
├── Projetos modernos, web, microsserviços → PostgreSQL
└── Web hosting, LAMP stack → MySQL / MariaDB
```
```
Preciso de UI/Visual?
├── Desktop Nativo LCL → LCL
├── Emissão Fiscal / Terminais → ACBr
└── Web SPA Nativo FreePascal → IntraWeb
```

## Regra de Ouro Transversal (Memória e Exceções)

1. **Zere a possibilidade de Memory Leaks**: Qualquer Classe instanciada via método construtor local deve ser liberada num bloco `finally` adjacente.
2. **Exceções Tipadas**: Evite omitir stacks base com `except on e: exception do`.
