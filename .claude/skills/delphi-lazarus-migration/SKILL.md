---
name: "Delphi IntraWeb to Lazarus Migration"
description: "Migracao Delphi IntraWeb -> Lazarus IntraWeb com equivalencia funcional, sem refatorar regra de negocio. Inclui conversao FireDAC->Zeos, DFM->LFM, ajuste de units Delphi para FPC e fluxo com checkpoint (RETOMAR/STATUS/PAUSAR)."
---

# Delphi IntraWeb to Lazarus Migration - Skill

Use esta skill quando o usuario pedir migracao Delphi -> Lazarus, port IntraWeb, conversao de projeto, ou continuidade de migracao interrompida.

## Activation Triggers

Ative esta skill quando houver termos como:
- migrar Delphi para Lazarus
- IntraWeb para Lazarus
- FireDAC para Zeos
- DFM para LFM
- System.* para Lazarus/FPC
- RETOMAR (em contexto de migracao)

## Non-Negotiable Rules

1. Nao refatorar logica de negocio.
2. Nao corrigir SQL, nomes de colunas ou tabelas.
3. Nao alterar comportamento funcional.
4. Nao criar arquivo .ini para conexao de banco.
5. Manter compatibilidade de runtime HTTP.SYS.
6. Atualizar migration_log.md a cada arquivo concluido.
7. Nunca pular etapa sem registrar no log.

## Output Contract

Sempre entregar:
- lista de arquivos migrados no ciclo atual
- resumo objetivo do que foi convertido
- riscos pendentes (se houver)
- proximo passo claro
- status atualizado em migration_log.md

## Checkpoint Workflow

Arquivo obrigatorio: migration_log.md na raiz do projeto.

Comandos de controle:
- RETOMAR: ler migration_log.md, localizar ultimo item concluido e continuar do proximo pendente.
- STATUS: exibir progresso por etapa, concluido/em andamento/pendente.
- PAUSAR: salvar estado atual e encerrar com seguranca.

Estrutura minima recomendada para o log:
- contexto do projeto
- etapa atual
- ultimo arquivo concluido
- lista pendente ordenada
- bloqueios
- proxima acao

## Migration Order

### Etapa 1 - Base do Projeto
- criar backup/, lib/, temp/, src/
- criar NomeProjeto.lpi
- criar NomeProjeto.lpr
- criar NomeProjeto.lres (vazio)
- inicializar migration_log.md

### Etapa 2 - Nucleo IntraWeb
Migrar nesta ordem:
1. servercontroller.pas/.lfm
2. usersessionunit.pas/.lfm
3. udm.pas/.lfm (converter FireDAC -> Zeos)
4. uBase.pas/.lfm (se existir)

### Etapa 3 - Restante da Aplicacao
Migrar formularios principais, depois secundarios, depois units auxiliares.

### Etapa 4 - Fechamento
- compilacao de teste
- verificacao de DLLs em lib/
- smoke test das funcionalidades criticas

## Mandatory Conversions

### Arquivos
- .dpr -> .lpr
- .dproj -> .lpi
- .dfm -> .lfm
- .pas -> .pas

### Banco de Dados
- TFDConnection -> TZConnection (unit ZConnection)
- TFDQuery -> TZQuery (unit ZDataset)
- TFDStoredProc -> TZStoredProc (unit ZDataset)
- TFDTable -> TZTable (unit ZDataset)

### Units Delphi para Lazarus/FPC
- System.SysUtils -> SysUtils
- System.Classes -> Classes
- System.JSON -> JsonDataObjects
- REST.Client/TRESTClient -> RESTRequest4D
- FireDAC.* -> ZConnection, ZDataset
- VCL.* -> LCL.* (ou remover quando nao aplicavel)
- System.Generics.Collections -> Generics.Collections

## Required Directives in Every .pas

Adicionar apos "unit NomeUnit;":

```pascal
{$mode delphiunicode}
{$codepage utf8}
```

## .lpr Baseline Template

```pascal
program NomeProjeto;

{$mode delphiunicode}
{$codepage utf8}

uses
  Interfaces,
  IWStartHSys,
  servercontroller,
  usersessionunit;

{$R *.res}

begin
  {$ifndef release}
  TIWStartHSys.Execute(True);
  {$else}
  TIWStartHSys.Execute(False);
  {$endif}
end.
```

## .lpi Required Packages

```xml
<RequiredPackages>
  <Item><PackageName Value="LazDaemon"/></Item>
  <Item><PackageName Value="LCL"/></Item>
  <Item><PackageName Value="FCL"/></Item>
  <Item><PackageName Value="dclIntraWeb_16_Lazarus"/></Item>
  <Item><PackageName Value="zcomponentdesign"/></Item>
  <Item><PackageName Value="zcomponent"/></Item>
</RequiredPackages>
```

## DB Connection Rule (No .ini)

Configurar conexao no fluxo de login/autenticacao, nao no startup do DataModule.

```pascal
procedure TDM.ConfigurarConexao(AServer, ADatabase, AUser, APass: string);
begin
  ZConnection1.Protocol := 'mysql';
  ZConnection1.HostName := AServer;
  ZConnection1.Database := ADatabase;
  ZConnection1.User := AUser;
  ZConnection1.Password := APass;
  ZConnection1.Port := 3306;
  ZConnection1.Connected := True;
end;
```

## Fast Troubleshooting

- Unit not found: revisar OtherUnitFiles no .lpi.
- Invalid compiler mode: confirmar diretivas mode/codepage.
- FireDAC symbol not found: revisar substituicao para Zeos e uses.
- RESTRequest4D/JsonDataObjects ausente: revisar paths de bibliotecas externas.
- erro em .lfm: abrir no Lazarus e salvar novamente.
- executavel nao inicia: conferir DLLs em lib/.

## Quality Gate Before Marking Done

Antes de concluir um lote de migracao, validar:
- compilacao sem erros de unidade faltante
- forms carregam sem erro de recurso
- fluxo de login e conexao funcionando
- migration_log.md atualizado

Se algum gate falhar, nao avancar para proxima etapa sem registrar no log.
