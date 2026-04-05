---
name: "Delphi to Lazarus Migration (Quick)"
description: "Versao curta para migracao Delphi/IntraWeb -> Lazarus com equivalencia funcional. Foco em checklist objetivo, sem refatoracao e com checkpoint de progresso."
---

# Delphi to Lazarus Migration (Quick)

Use quando o usuario quiser migracao rapida e objetiva de Delphi para Lazarus, incluindo IntraWeb, FireDAC -> Zeos ou DFM -> LFM.

## Trigger Rapido

- "migrar Delphi para Lazarus"
- "converter IntraWeb"
- "trocar FireDAC por Zeos"
- "converter DFM para LFM"
- "RETOMAR" em contexto de migracao

## Regras Inegociaveis

1. Nao refatorar logica.
2. Nao corrigir SQL ou nomes de schema.
3. Nao alterar funcionalidade.
4. Nao criar .ini de banco.
5. Atualizar migration_log.md apos cada arquivo.

## Checklist Minimo

1. Ler/criar migration_log.md.
2. Migrar arquivos-base: .dpr -> .lpr, .dproj -> .lpi, .dfm -> .lfm.
3. Adicionar em cada .pas:

```pascal
{$mode delphiunicode}
{$codepage utf8}
```

4. Converter FireDAC -> Zeos (TFDConnection/Query/etc para TZ*).
5. Ajustar units Delphi (System.* / VCL.*) para FPC/Lazarus.
6. Compilar e registrar status final no log.

## Ordem Recomendada

1. servercontroller
2. usersessionunit
3. udm (DB)
4. demais forms/units

## Resultado Esperado

Entregar sempre:
- arquivos convertidos
- pendencias/riscos
- proximo passo
- status do migration_log.md
