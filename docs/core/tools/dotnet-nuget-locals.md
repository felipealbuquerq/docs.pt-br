---
title: Comando dotnet nuget locals
description: O comando nuget dotnet locals limpa ou lista os recursos locais do NuGet, como cache de solicitação http-, cache temporário ou pasta de pacotes globais em todo o computador.
author: karann-msft
ms.date: 06/26/2019
ms.openlocfilehash: 0cf025f91a7582fafc401799cd1d8b933b087535
ms.sourcegitcommit: 2d792961ed48f235cf413d6031576373c3050918
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70202473"
---
# <a name="dotnet-nuget-locals"></a>dotnet nuget locals

**Este tópico aplica-se a: ✓** SDK do .NET Core 1.x e versões posteriores

<!-- todo: uncomment when all CLI commands are reviewed
[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]
-->

## <a name="name"></a>Nome

`dotnet nuget locals`-Limpa ou lista os recursos locais do NuGet.

## <a name="synopsis"></a>Sinopse

```console
dotnet nuget locals <CACHE_LOCATION> [(-c|--clear)|(-l|--list)] [--force-english-output]
dotnet nuget locals [-h|--help]
```

## <a name="description"></a>Descrição

O comando `dotnet nuget locals` limpa ou lista os recursos locais do NuGet no cache de solicitação http, cache temporário ou pasta de pacotes globais em todo o computador.

## <a name="arguments"></a>Arguments

* **`CACHE_LOCATION`**

  O local do cache a ser listado ou limpo. Aceita um dos seguintes valores:

  * `all` – Indica que a operação especificada aplica-se a todos os tipos cache: o cache de solicitação http, cache de pacotes globais e o cache temporário.
  * `http-cache` – Indica que a operação especificada aplica-se apenas ao cache de solicitação http. Os outros locais do cache não são afetados.
  * `global-packages` – Indica que a operação especificada aplica-se apenas ao cache de pacotes globais. Os outros locais do cache não são afetados.
  * `temp` – Indica que a operação especificada aplica-se apenas ao cache temporário. Os outros locais do cache não são afetados.

## <a name="options"></a>Opções

* **`--force-english-output`**

  Força a execução do aplicativo usando uma cultura invariável com base em inglês.

* **`-h|--help`**

  Imprime uma ajuda breve para o comando.

* **`-c|--clear`**

  A opção clear executa uma operação de limpeza no tipo de cache especificado. O conteúdo dos diretórios de cache é excluído recursivamente. O usuário/grupo executor deve ter permissão nos arquivos nos diretórios de cache. Caso contrário, um erro é exibido, indicando as pastas/os arquivos que não foram limpos.

* **`-l|--list`**

  A opção de lista é usada para exibir a localização do tipo de cache especificado.

## <a name="examples"></a>Exemplos

* Exibe os caminhos de todos os diretórios de cache local (diretório de cache de http, o diretório de cache de pacotes globais e diretório de cache temporário):

  ```console
  dotnet nuget locals –l all
  ```

* Exibe o caminho para o diretório local do cache de http:

  ```console
  dotnet nuget locals --list http-cache
  ```

* Limpa todos os arquivos dos diretórios de cache local (diretório de cache de http, o diretório de cache de pacotes globais e diretório de cache temporário):

  ```console
  dotnet nuget locals --clear all
  ```

* Limpa todos os arquivos no diretório local de cache de pacotes globais:

  ```console
  dotnet nuget locals -c global-packages
  ```

* Limpa todos os arquivos no diretório local de cache temporário:

  ```console
  dotnet nuget locals -c temp
  ```

## <a name="troubleshooting"></a>Solução de problemas

Para saber mais sobre os problemas e erros mais comuns ao usar o comando `dotnet nuget locals`, veja [Gerenciamento do cache do NuGet](/nuget/consume-packages/managing-the-nuget-cache).
