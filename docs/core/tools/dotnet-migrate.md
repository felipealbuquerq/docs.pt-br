---
title: Comando dotnet migrate
description: O comando dotnet migrate migra um projeto e todas as suas dependências.
ms.date: 06/26/2019
ms.openlocfilehash: 86f11592e774da12b010886aaa1e30cee063fea6
ms.sourcegitcommit: 2d792961ed48f235cf413d6031576373c3050918
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70202540"
---
# <a name="dotnet-migrate"></a>dotnet migrate

**Este tópico aplica-se a: ✓** SDK do .NET Core 1.x e versões posteriores

<!-- todo: uncomment when all CLI commands are reviewed
[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]
-->

## <a name="name"></a>Nome

`dotnet migrate` – Migrar um projeto do .NET Core Versão Prévia 2 para um projeto no estilo do SDK do .NET Core.

> [!NOTE]
> `dotnet migrate` será removido do SDK do .NET Core 3.0 na próxima versão prévia.

## <a name="synopsis"></a>Sinopse

```console
dotnet migrate [<SOLUTION_FILE|PROJECT_DIR>] [--format-report-file-json] [-r|--report-file] [-s|--skip-project-references] [--skip-backup] [-t|--template-file] [-v|--sdk-package-version] [-x|--xproj-file]
dotnet migrate [-h|--help]
```

## <a name="description"></a>DESCRIÇÃO

O comando `dotnet migrate` migra um projeto válido baseado em *project.json* da Versão Prévia 2 para um projeto *csproj* válido no estilo do SDK do .NET Core.

Por padrão, o comando migra o projeto raiz e as referências de projeto que o projeto raiz contém. Esse comportamento é desabilitado usando a opção `--skip-project-references` no tempo de execução.

A migração pode ser realizada nos seguintes ativos:

* Um único projeto, especificando o arquivo *project.json* a ser migrado.
* Todos os diretórios especificados no arquivo *global.json*, passando um caminho para o arquivo *global.json*.
* O arquivo *solution.sln*, onde ele migra os projetos referenciados na solução.
* Em todos os subdiretórios do diretório especificado, recursivamente.

O comando `dotnet migrate` mantém o arquivo *project.json* migrado em um diretório `backup`, que será criado se ainda não existir. Esse comportamento é substituído com a opção `--skip-backup`.

Por padrão, a operação de migração produzirá o estado do processo de migração para a saída padrão (STDOUT). Se você usar a opção `--report-file <REPORT_FILE>`, a saída será salva no arquivo especificado.

O comando `dotnet migrate` dá suporte apenas a projetos com base em *project.json* da Visualização 2. Isso significa que você não pode usá-lo para migrar projetos com base em *project.json* de DNX ou Visualização 1 diretamente para projetos de MSBuild/csproj. Primeiro, você precisa migrar manualmente o projeto para um projeto com base em *project.json* de Visualização 2 e, em seguida, usar o comando `dotnet migrate` para migrar o projeto.

## <a name="arguments"></a>Arguments

`PROJECT_JSON/GLOBAL_JSON/SOLUTION_FILE/PROJECT_DIR`

O caminho até um dos seguintes:

* um arquivo *project.json* a ser migrado.
* um arquivo *global.json*: as pastas especificadas em *global.json* são migradas.
* um arquivo *solution.sln*: os projetos referenciados na solução são migrados.
* um diretório a ser migrado: pesquisa recursivamente arquivos *project.json* para fazer a migração dentro do diretório especificado.

Se nada for especificado, o padrão será o diretório atual.

## <a name="options"></a>Opções

`--format-report-file-json <REPORT_FILE>`

Arquivo de relatório de migração de saída como JSON em vez de mensagens de usuário.

`-h|--help`

Imprime uma ajuda breve para o comando.

`-r|--report-file <REPORT_FILE>`

Relatório de migração de saída em um arquivo além do console.

`-s|--skip-project-references [Debug|Release]`

Ignorar referências de projeto migradas. Por padrão, as referências de projeto são migradas recursivamente.

`--skip-backup`

Ignorar a movimentação de *project.json*, *global.json* e *\*.xproj* para um diretório `backup` após a migração bem-sucedida.

`-t|--template-file <TEMPLATE_FILE>`

Arquivo csproj de modelo a ser usado para migração. Por padrão, o mesmo modelo removido por `dotnet new console` será usado.

`-v|--sdk-package-version <VERSION>`

A versão do pacote do sdk referenciada no aplicativo migrado. O padrão é a versão do SDK no `dotnet new`.

`-x|--xproj-file <FILE>`

O caminho para o arquivo xproj a ser usado. Necessário quando há mais de um xproj em um diretório do projeto.

## <a name="examples"></a>Exemplos

Migre um projeto no diretório atual e todas as dependências projeto a projeto:

`dotnet migrate`

Migre todos os projetos incluídos pelo arquivo *global.json*:

`dotnet migrate path/to/global.json`

Migre apenas o projeto atual e não as dependências P2P (projeto a projeto). Além disso, use uma versão específica do SDK:

`dotnet migrate -s -v 1.0.0-preview4`
