---
title: Comando dotnet new
description: O comando dotnet new cria novos projetos .NET Core com base no modelo especificado.
ms.date: 05/06/2019
ms.openlocfilehash: c9e960bab0e28e88b0cc8d431dad3b9f3f00c9c0
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69660549"
---
# <a name="dotnet-new"></a>dotnet new

[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]

## <a name="name"></a>Nome

`dotnet new` – Cria um novo projeto, arquivo de configuração ou solução com base no modelo especificado.

## <a name="synopsis"></a>Sinopse

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

```console
dotnet new <TEMPLATE> [--dry-run] [--force] [-i|--install] [-lang|--language] [-n|--name] [--nuget-source] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

```console
dotnet new <TEMPLATE> [--force] [-i|--install] [-lang|--language] [-n|--name] [--nuget-source] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

```console
dotnet new <TEMPLATE> [--force] [-i|--install] [-lang|--language] [-n|--name] [-o|--output] [-u|--uninstall] [Template options]
dotnet new <TEMPLATE> [-l|--list] [--type]
dotnet new [-h|--help]
```

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

```console
dotnet new <TEMPLATE> [-lang|--language] [-n|--name] [-o|--output] [-all|--show-all] [-h|--help] [Template options]
dotnet new <TEMPLATE> [-l|--list]
dotnet new [-all|--show-all]
dotnet new [-h|--help]
```

---

## <a name="description"></a>DESCRIÇÃO

O comando `dotnet new` fornece uma maneira conveniente de inicializar um projeto .NET Core válido.

O comando chama o [mecanismo de modelo](https://github.com/dotnet/templating) para criar os artefatos em disco com base no modelo e nas opções especificadas.

## <a name="arguments"></a>Arguments

`TEMPLATE`

O modelo para o qual criar uma instância quando o comando é invocado. Cada modelo pode ter opções específicas que podem ser passadas. Para obter mais informações, consulte [Opções de modelo](#template-options).

Se o valor `TEMPLATE` não for uma correspondência exata no texto na coluna **Modelos** ou **Nome Curto**, uma correspondência de substring será executada nessas duas colunas.

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

O comando contém uma lista padrão de modelos. Use `dotnet new -l` para obter uma lista dos modelos disponíveis. A tabela a seguir mostra os modelos que vêm pré-instalados com o SDK do .NET Core 2.2.100. O idioma padrão do modelo é mostrado entre parênteses.

| Modelos                                    | Nome curto        | Idioma     | Marcas                                  |
|----------------------------------------------|-------------------|--------------|---------------------------------------|
| Aplicativo do Console                          | `console`         | [C#], F#, VB | Comum/Console                        |
| Biblioteca de classes                                | `classlib`        | [C#], F#, VB | Comum/Library                        |
| Projeto de Teste de Unidade                            | `mstest`          | [C#], F#, VB | Teste/MSTest                           |
| Projeto de Teste do NUnit 3                         | `nunit`           | [C#], F#, VB | Teste/NUnit                            |
| Item de Teste do NUnit 3                            | `nunit-test`      | [C#], F#, VB | Teste/NUnit                            |
| Projeto de Teste xUnit                           | `xunit`           | [C#], F#, VB | Teste/xUnit                            |
| Página do Razor                                   | `page`            | [C#]         | Web/ASP.NET                           |
| Importações de Exibição do MVC                              | `viewimports`     | [C#]         | Web/ASP.NET                           |
| MVC ViewStart                                | `viewstart`       | [C#]         | Web/ASP.NET                           |
| ASP.NET Core Vazio                           | `web`             | [C#], F#     | Web/Vazio                             |
| Aplicativo Web ASP.NET Core (Modelo-Exibição-Controlador) | `mvc`             | [C#], F#     | Web/MVC                               |
| Aplicativo Web ASP.NET Core                         | `webapp`, `razor` | [C#]         | Web/MVC/Razor Pages                   |
| ASP.NET Core com Angular                    | `angular`         | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core com React.js                   | `react`           | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core com React.js e Redux         | `reactredux`      | [C#]         | Web/MVC/SPA                           |
| Biblioteca de Classes do Razor                          | `razorclasslib`   | [C#]         | Web/Razor/Biblioteca/Biblioteca de Classes do Razor |
| API Web do ASP.NET Core                         | `webapi`          | [C#], F#     | Web/WebAPI                            |
| Arquivo global.json                             | `globaljson`      |              | Config                                |
| Configuração do NuGet                                 | `nugetconfig`     |              | Config                                |
| Configuração da Web                                   | `webconfig`       |              | Config                                |
| Arquivo de Solução                                | `sln`             |              | Solução                              |

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

O comando contém uma lista padrão de modelos. Use `dotnet new -l` para obter uma lista dos modelos disponíveis. A tabela a seguir mostra os modelos que vêm pré-instalados com o SDK do .NET Core 2.1.300. O idioma padrão do modelo é mostrado entre parênteses.

| Modelos                                    | Nome curto      | Idioma     | Marcas                                  |
|----------------------------------------------|-----------------|--------------|---------------------------------------|
| Aplicativo do Console                          | `console`       | [C#], F#, VB | Comum/Console                        |
| Biblioteca de classes                                | `classlib`      | [C#], F#, VB | Comum/Library                        |
| Projeto de Teste de Unidade                            | `mstest`        | [C#], F#, VB | Teste/MSTest                           |
| Projeto de Teste xUnit                           | `xunit`         | [C#], F#, VB | Teste/xUnit                            |
| Página do Razor                                   | `page`          | [C#]         | Web/ASP.NET                           |
| Importações de Exibição do MVC                              | `viewimports`   | [C#]         | Web/ASP.NET                           |
| MVC ViewStart                                | `viewstart`     | [C#]         | Web/ASP.NET                           |
| ASP.NET Core Vazio                           | `web`           | [C#], F#     | Web/Vazio                             |
| Aplicativo Web ASP.NET Core (Modelo-Exibição-Controlador) | `mvc`           | [C#], F#     | Web/MVC                               |
| Aplicativo Web ASP.NET Core                         | `razor`         | [C#]         | Web/MVC/Razor Pages                   |
| ASP.NET Core com Angular                    | `angular`       | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core com React.js                   | `react`         | [C#]         | Web/MVC/SPA                           |
| ASP.NET Core com React.js e Redux         | `reactredux`    | [C#]         | Web/MVC/SPA                           | 
| Biblioteca de Classes do Razor                          | `razorclasslib` | [C#]         | Web/Razor/Biblioteca/Biblioteca de Classes do Razor |
| API Web do ASP.NET Core                         | `webapi`        | [C#], F#     | Web/WebAPI                            |
| Arquivo global.json                             | `globaljson`    |              | Config                                |
| Configuração do NuGet                                 | `nugetconfig`   |              | Config                                |
| Configuração da Web                                   | `webconfig`     |              | Config                                |
| Arquivo de Solução                                | `sln`           |              | Solução                              |

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

O comando contém uma lista padrão de modelos. Use `dotnet new -l` para obter uma lista dos modelos disponíveis. A tabela a seguir mostra os modelos que vêm pré-instalados com o SDK do .NET Core 2.0.0. O idioma padrão do modelo é mostrado entre parênteses.

| Modelos                                    | Nome curto    | Idioma     | Marcas                |
|----------------------------------------------|---------------|--------------|---------------------|
| Aplicativo do Console                          | `console`     | [C#], F#, VB | Comum/Console      |
| Biblioteca de classes                                | `classlib`    | [C#], F#, VB | Comum/Library      |
| Projeto de Teste de Unidade                            | `mstest`      | [C#], F#, VB | Teste/MSTest         |
| Projeto de Teste xUnit                           | `xunit`       | [C#], F#, VB | Teste/xUnit          |
| ASP.NET Core Vazio                           | `web`         | [C#], F#     | Web/Vazio           |
| Aplicativo Web ASP.NET Core (Modelo-Exibição-Controlador) | `mvc`         | [C#], F#     | Web/MVC             |
| Aplicativo Web ASP.NET Core                         | `razor`       | [C#]         | Web/MVC/Razor Pages |
| ASP.NET Core com Angular                    | `angular`     | [C#]         | Web/MVC/SPA         |
| ASP.NET Core com React.js                   | `react`       | [C#]         | Web/MVC/SPA         |
| ASP.NET Core com React.js e Redux         | `reactredux`  | [C#]         | Web/MVC/SPA         |
| API Web do ASP.NET Core                         | `webapi`      | [C#], F#     | Web/WebAPI          |
| Arquivo global.json                             | `globaljson`  |              | Config              |
| Configuração do Nuget                                 | `nugetconfig` |              | Config              |
| Configuração da Web                                   | `webconfig`   |              | Config              |
| Arquivo de Solução                                | `sln`         |              | Solução            |
| Página do Razor                                   | `page`        |              | Web/ASP.NET         |
| Importações de Exibição do MVC                              | `viewimports` |              | Web/ASP.NET         |
| MVC ViewStart                                | `viewstart`   |              | Web/ASP.NET         |

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

O comando contém uma lista padrão de modelos. Use `dotnet new -all` para obter uma lista dos modelos disponíveis. A tabela a seguir mostra os modelos que vêm pré-instalados com o SDK do .NET Core 1.0.1. O idioma padrão do modelo é mostrado entre parênteses.

| Modelos            | Nome curto    | Idioma | Marcas           |
|----------------------|---------------|----------|----------------|
| Aplicativo do Console  | `console`     | [C#], F# | Comum/Console |
| Biblioteca de classes        | `classlib`    | [C#], F# | Comum/Library |
| Projeto de Teste de Unidade    | `mstest`      | [C#], F# | Teste/MSTest    |
| Projeto de Teste xUnit   | `xunit`       | [C#], F# | Teste/xUnit     |
| ASP.NET Core Vazio   | `web`         | [C#]     | Web/Vazio      |
| Aplicativo Web ASP.NET Core | `mvc`         | [C#], F# | Web/MVC        |
| API Web do ASP.NET Core | `webapi`      | [C#]     | Web/WebAPI     |
| Configuração do Nuget         | `nugetconfig` |          | Config         |
| Configuração da Web           | `webconfig`   |          | Config         |
| Arquivo de Solução        | `sln`         |          | Solução       |

---

## <a name="options"></a>Opções

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

`--dry-run`

Exibe um resumo do que ocorreria se o comando fornecido fosse executado se resultasse na criação de um modelo.

`--force`

Força o conteúdo a ser gerado mesmo se ele alterasse os arquivos existentes. Isso é necessário quando o diretório de saída já contiver um projeto.

`-h|--help`

Imprime uma ajuda para o comando. Ele pode ser invocado para o próprio comando `dotnet new` ou para qualquer modelo, como `dotnet new mvc --help`.

`-i|--install <PATH|NUGET_ID>`

Instala um pacote de origem ou de modelo do `PATH` ou `NUGET_ID` fornecido. Se você deseja instalar uma versão de pré-lançamento de um pacote de modelo, é necessário especificar a versão no formato `<package-name>::<package-version>`. Por padrão, `dotnet new` passa \* para a versão, o que representa a última versão estável do pacote. Veja um exemplo na seção [Exemplos](#examples).

Para obter informações sobre a criação de modelos personalizados, consulte [Custom templates for dotnet new](custom-templates.md) (Modelos personalizados para dotnet new).

`-l|--list`

Lista os modelos que contêm o nome especificado. Se for invocado para o comando `dotnet new`, listará os possíveis modelos disponíveis para o diretório especificado. Por exemplo, se o diretório já contiver um projeto, ele não listará todos os modelos de projeto.

`-lang|--language {C#|F#|VB}`

A linguagem do modelo a ser criada. A linguagem aceita varia de acordo com o modelo (consulte os padrões na seção [Argumentos](#arguments)). Não é válida para alguns modelos.

> [!NOTE]
> Alguns shells interpretam `#` como um caractere especial. Nesses casos, você precisa acrescentar o valor do parâmetro de idioma, como `dotnet new console -lang "F#"`.

`-n|--name <OUTPUT_NAME>`

O nome para a saída criada. Se nenhum nome for especificado, o nome do diretório atual será usado.

`--nuget-source`

Especifica uma origem do NuGet a ser usada durante a instalação.

`-o|--output <OUTPUT_DIRECTORY>`

Local para colocar a saída gerada. O padrão é o diretório atual.

`--type`

Filtra modelos com base em tipos disponíveis. Os valores predefinidos são "projeto", "item" ou "outro".

`-u|--uninstall <PATH|NUGET_ID>`

Desinstala um pacote de origem ou de modelo no `PATH` ou `NUGET_ID` fornecido. Ao excluir o valor `<PATH|NUGET_ID>`, todos os pacotes de modelo atualmente instalados e seus modelos associados são exibidos.

> [!NOTE]
> Para desinstalar um modelo usando um `PATH`, você precisa qualificar totalmente o caminho. Por exemplo, *C:/Usuários/\<USUÁRIO>/Documentos/Modelos/GarciaSoftware.ConsoleTemplate.CSharp* funcionará, mas *./GarciaSoftware.ConsoleTemplate.CSharp* da pasta que o contém, não.
> Além disso, não inclua uma barra final de encerramento de diretório no caminho do modelo.

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

`--force`

Força o conteúdo a ser gerado mesmo se ele alterasse os arquivos existentes. Isso é necessário quando o diretório de saída já contiver um projeto.

`-h|--help`

Imprime uma ajuda para o comando. Ele pode ser invocado para o próprio comando `dotnet new` ou para qualquer modelo, como `dotnet new mvc --help`.

`-i|--install <PATH|NUGET_ID>`

Instala um pacote de origem ou de modelo do `PATH` ou `NUGET_ID` fornecido. Se você deseja instalar uma versão de pré-lançamento de um pacote de modelo, é necessário especificar a versão no formato `<package-name>::<package-version>`. Por padrão, `dotnet new` passa \* para a versão, o que representa a última versão estável do pacote. Veja um exemplo na seção [Exemplos](#examples).

Para obter informações sobre a criação de modelos personalizados, consulte [Custom templates for dotnet new](custom-templates.md) (Modelos personalizados para dotnet new).

`-l|--list`

Lista os modelos que contêm o nome especificado. Se for invocado para o comando `dotnet new`, listará os possíveis modelos disponíveis para o diretório especificado. Por exemplo, se o diretório já contiver um projeto, ele não listará todos os modelos de projeto.

`-lang|--language {C#|F#|VB}`

A linguagem do modelo a ser criada. A linguagem aceita varia de acordo com o modelo (consulte os padrões na seção [Argumentos](#arguments)). Não é válida para alguns modelos.

> [!NOTE]
> Alguns shells interpretam `#` como um caractere especial. Nesses casos, você precisa acrescentar o valor do parâmetro de idioma, como `dotnet new console -lang "F#"`.

`-n|--name <OUTPUT_NAME>`

O nome para a saída criada. Se nenhum nome for especificado, o nome do diretório atual será usado.

`--nuget-source`

Especifica uma origem do NuGet a ser usada durante a instalação.

`-o|--output <OUTPUT_DIRECTORY>`

Local para colocar a saída gerada. O padrão é o diretório atual.

`--type`

Filtra modelos com base em tipos disponíveis. Os valores predefinidos são "projeto", "item" ou "outro".

`-u|--uninstall <PATH|NUGET_ID>`

Desinstala um pacote de origem ou de modelo no `PATH` ou `NUGET_ID` fornecido.

> [!NOTE]
> Para desinstalar um modelo usando um `PATH`, você precisa qualificar totalmente o caminho. Por exemplo, *C:/Usuários/\<USUÁRIO>/Documentos/Modelos/GarciaSoftware.ConsoleTemplate.CSharp* funcionará, mas *./GarciaSoftware.ConsoleTemplate.CSharp* da pasta que o contém, não.
> Além disso, não inclua uma barra final de encerramento de diretório no caminho do modelo.

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

`--force`

Força o conteúdo a ser gerado mesmo se ele alterasse os arquivos existentes. Isso é necessário quando o diretório de saída já contiver um projeto.

`-h|--help`

Imprime uma ajuda para o comando. Ele pode ser invocado para o próprio comando `dotnet new` ou para qualquer modelo, como `dotnet new mvc --help`.

`-i|--install <PATH|NUGET_ID>`

Instala um pacote de origem ou de modelo do `PATH` ou `NUGET_ID` fornecido. Se você deseja instalar uma versão de pré-lançamento de um pacote de modelo, é necessário especificar a versão no formato `<package-name>::<package-version>`. Por padrão, `dotnet new` passa \* para a versão, o que representa a última versão estável do pacote. Veja um exemplo na seção [Exemplos](#examples).

Para obter informações sobre a criação de modelos personalizados, consulte [Custom templates for dotnet new](custom-templates.md) (Modelos personalizados para dotnet new).

`-l|--list`

Lista os modelos que contêm o nome especificado. Se for invocado para o comando `dotnet new`, listará os possíveis modelos disponíveis para o diretório especificado. Por exemplo, se o diretório já contiver um projeto, ele não listará todos os modelos de projeto.

`-lang|--language {C#|F#|VB}`

A linguagem do modelo a ser criada. A linguagem aceita varia de acordo com o modelo (consulte os padrões na seção [Argumentos](#arguments)). Não é válida para alguns modelos.

> [!NOTE]
> Alguns shells interpretam `#` como um caractere especial. Nesses casos, você precisa acrescentar o valor do parâmetro de idioma, como `dotnet new console -lang "F#"`.

`-n|--name <OUTPUT_NAME>`

O nome para a saída criada. Se nenhum nome for especificado, o nome do diretório atual será usado.

`-o|--output <OUTPUT_DIRECTORY>`

Local para colocar a saída gerada. O padrão é o diretório atual.

`--type`

Filtra modelos com base em tipos disponíveis. Os valores predefinidos são "projeto", "item" ou "outro".

`-u|--uninstall <PATH|NUGET_ID>`

Desinstala um pacote de origem ou de modelo no `PATH` ou `NUGET_ID` fornecido.

> [!NOTE]
> Para desinstalar um modelo usando uma origem `PATH`, você precisa qualificar totalmente o caminho. Por exemplo, *C:/Usuários/\<USUÁRIO>/Documentos/Modelos/GarciaSoftware.ConsoleTemplate.CSharp* funcionará, mas *./GarciaSoftware.ConsoleTemplate.CSharp* da pasta que o contém, não. Além disso, não inclua uma barra final de encerramento de diretório no caminho do modelo.
> 
> Se não for possível determinar o argumento `PATH` ou `NUGET_ID` necessário para desinstalar um modelo, executar `dotnet new --uninstall` sem um argumento listará todos os modelos instalados e o argumento necessário desinstalá-los.

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

`-all|--show-all`

Mostra todos os modelos de um tipo específico de projeto durante a execução no contexto do comando `dotnet new` isolado. Durante a execução no contexto de um modelo específico, como `dotnet new web -all`, `-all` é interpretado como um sinalizador de criação de força. Isso é necessário quando o diretório de saída já contiver um projeto.

`-h|--help`

Imprime uma ajuda para o comando. Ele pode ser invocado para o próprio comando `dotnet new` ou para qualquer modelo, como `dotnet new mvc --help`.

`-l|--list`

Lista os modelos que contêm o nome especificado. Se for invocado para o comando `dotnet new`, listará os possíveis modelos disponíveis para o diretório especificado. Por exemplo, se o diretório já contiver um projeto, ele não listará todos os modelos de projeto.

`-lang|--language {C#|F#}`

A linguagem do modelo a ser criada. A linguagem aceita varia de acordo com o modelo (consulte os padrões na seção [Argumentos](#arguments)). Não é válida para alguns modelos.

> [!NOTE]
> Alguns shells interpretam `#` como um caractere especial. Nesses casos, você precisa acrescentar o valor do parâmetro de idioma, como `dotnet new console -lang "F#"`.

`-n|--name <OUTPUT_NAME>`

O nome para a saída criada. Se nenhum nome for especificado, o nome do diretório atual será usado.

`-o|--output <OUTPUT_DIRECTORY>`

Local para colocar a saída gerada. O padrão é o diretório atual.

---

## <a name="template-options"></a>Opções de modelo

Cada modelo de projeto pode ter opções adicionais disponíveis. Os principais modelos têm as seguintes opções adicionais:

# <a name="net-core-22tabnetcore22"></a>[.NET Core 2.2](#tab/netcore22)

**console**

`--langVersion <VERSION_NUMBER>` – Define a propriedade `LangVersion` no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3. Sem suporte para F#.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**angular, react, reactredux**

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

`--no-https` – O projeto não requer HTTPS. Essa opção é aplicável somente se `IndividualAuth` ou `OrganizationalAuth` não está sendo usado.

**razorclasslib**

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**classlib**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp2.2` para criar uma Biblioteca de classes do .NET Core ou `netstandard2.0` para criar uma Biblioteca de classes .NET Standard. O valor padrão é `netstandard2.0`.

`--langVersion <VERSION_NUMBER>` – Define a propriedade `LangVersion` no arquivo de projeto criado. Por exemplo, use `--langVersion 7.3` para usar C# 7.3. Sem suporte para F#.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**mstest, xunit**

`-p|--enable-pack` – Habilita empacotamento para o projeto usando [dotnet pack](dotnet-pack.md).

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**nunit**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. O valor padrão é `netcoreapp2.1`.

`-p|--enable-pack` – Habilita empacotamento para o projeto usando [dotnet pack](dotnet-pack.md).

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**page**

`-na|--namespace <NAMESPACE_NAME>` – namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

`-np|--no-pagemodel` – Cria a página sem um PageModel.

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` – namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

**web**

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

`--no-https` – O projeto não requer HTTPS. Essa opção é aplicável somente se `IndividualAuth` ou `OrganizationalAuth` não está sendo usado.

**mvc, webapp**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `Individual` – Autenticação individual.
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `MultiOrg` – Autenticação organizacional para vários locatários.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`-rp|--reset-password-policy-id <ID>` – A ID da política de senha de redefinição para este projeto. Use com a autenticação do `IndividualB2C`.

`-ep|--edit-profile-policy-id <ID>` – A ID da política de perfil de edição para este projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação `SingleOrg` ou `MultiOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C`, `SingleOrg` ou `MultiOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`--callback-path <PATH>` – O caminho da solicitação dentro do caminho base do aplicativo do URI de redirecionamento. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `/signin-oidc`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--no-https` – O projeto não requer HTTPS. `app.UseHsts` e `app.UseHttpsRedirection` não são adicionados ao `Startup.Configure`. Essa opção é aplicável somente se `Individual`, `IndividualB2C`, `SingleOrg` ou `MultiOrg` não estão sendo usados.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C` ou `SingleOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--no-https` – O projeto não requer HTTPS. `app.UseHsts` e `app.UseHttpsRedirection` não são adicionados ao `Startup.Configure`. Essa opção é aplicável somente se `Individual`, `IndividualB2C`, `SingleOrg` ou `MultiOrg` não estão sendo usados.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**globaljson**

`--sdk-version <VERSION_NUMBER>` – Especifica a versão do SDK do .NET Core a ser usada no arquivo *global.json*.

# <a name="net-core-21tabnetcore21"></a>[.NET Core 2.1](#tab/netcore21)

**console, angular, react, reactredux, razorclasslib**

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**classlib**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp2.1` para criar uma Biblioteca de classes do .NET Core ou `netstandard2.0` para criar uma Biblioteca de classes .NET Standard. O valor padrão é `netstandard2.0`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**mstest, xunit**

`-p|--enable-pack` – Habilita empacotamento para o projeto usando [dotnet pack](dotnet-pack.md).

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**globaljson**

`--sdk-version <VERSION_NUMBER>` – Especifica a versão do SDK do .NET Core a ser usada no arquivo *global.json*.

**web**

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

`--no-https` – O projeto não requer HTTPS. Essa opção é aplicável somente se `IndividualAuth` ou `OrganizationalAuth` não está sendo usado.

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C` ou `SingleOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

`--no-https` – O projeto não requer HTTPS. `app.UseHsts` e `app.UseHttpsRedirection` não são adicionados ao `Startup.Configure`. Essa opção é aplicável somente se `Individual`, `IndividualB2C`, `SingleOrg` ou `MultiOrg` não estão sendo usados.

**mvc, razor**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `Individual` – Autenticação individual.
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `MultiOrg` – Autenticação organizacional para vários locatários.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`-rp|--reset-password-policy-id <ID>` – A ID da política de senha de redefinição para este projeto. Use com a autenticação do `IndividualB2C`.

`-ep|--edit-profile-policy-id <ID>` – A ID da política de perfil de edição para este projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação `SingleOrg` ou `MultiOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C`, `SingleOrg` ou `MultiOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`--callback-path <PATH>` – O caminho da solicitação dentro do caminho base do aplicativo do URI de redirecionamento. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `/signin-oidc`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--exclude-launch-settings` – Excluir *launchSettings.json* do modelo gerado.

`--use-browserlink` – Inclui BrowserLink no projeto.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

`--no-https` – O projeto não requer HTTPS. `app.UseHsts` e `app.UseHttpsRedirection` não são adicionados ao `Startup.Configure`. Essa opção é aplicável somente se `Individual`, `IndividualB2C`, `SingleOrg` ou `MultiOrg` não estão sendo usados.

**page**

`-na|--namespace <NAMESPACE_NAME>` – namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

`-np|--no-pagemodel` – Cria a página sem um PageModel.

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` – namespace para o código gerado. O valor padrão é `MyApp.Namespace`.

# <a name="net-core-20tabnetcore20"></a>[.NET Core 2.0](#tab/netcore20)

**console, angular, react, reactredux**

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**classlib**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp2.0` para criar uma Biblioteca de classes do .NET Core ou `netstandard2.0` para criar uma Biblioteca de classes .NET Standard. O valor padrão é `netstandard2.0`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**mstest, xunit**

`-p|--enable-pack` – Habilita empacotamento para o projeto usando [dotnet pack](dotnet-pack.md).

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**globaljson**

`--sdk-version <VERSION_NUMBER>` – Especifica a versão do SDK do .NET Core a ser usada no arquivo *global.json*.

**web**

`--use-launch-settings` – Inclui *launchSettings.json* na saída do modelo gerado.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**webapi**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C` ou `SingleOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--use-launch-settings` – Inclui *launchSettings.json* na saída do modelo gerado.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**mvc, razor**

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Os valores possíveis são:

- `None` – Nenhuma autenticação (Padrão).
- `Individual` – Autenticação individual.
- `IndividualB2C` – Autenticação individual com o Azure AD B2C.
- `SingleOrg` – Autenticação organizacional para um único locatário.
- `MultiOrg` – Autenticação organizacional para vários locatários.
- `Windows` – Autenticação do Windows.

`--aad-b2c-instance <INSTANCE>` – A instância do Azure Active Directory B2C à qual se conectar. Use com a autenticação do `IndividualB2C`. O valor padrão é `https://login.microsoftonline.com/tfp/`.

`-ssp|--susi-policy-id <ID>` – A ID da política de credenciais e de inscrição desse projeto. Use com a autenticação do `IndividualB2C`.

`-rp|--reset-password-policy-id <ID>` – A ID da política de senha de redefinição para este projeto. Use com a autenticação do `IndividualB2C`.

`-ep|--edit-profile-policy-id <ID>` – A ID da política de perfil de edição para este projeto. Use com a autenticação do `IndividualB2C`.

`--aad-instance <INSTANCE>` – A instância do Azure Active Directory à qual se conectar. Use com a autenticação `SingleOrg` ou `MultiOrg`. O valor padrão é `https://login.microsoftonline.com/`.

`--client-id <ID>` – A ID do cliente deste projeto. Use com a autenticação `IndividualB2C`, `SingleOrg` ou `MultiOrg`. O valor padrão é `11111111-1111-1111-11111111111111111`.

`--domain <DOMAIN>` – O domínio do locatário de diretório. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `qualified.domain.name`.

`--tenant-id <ID>` – A ID TenantId do diretório ao qual se conectar. Use com a autenticação do `SingleOrg`. O valor padrão é `22222222-2222-2222-2222-222222222222`.

`--callback-path <PATH>` – O caminho da solicitação dentro do caminho base do aplicativo do URI de redirecionamento. Use com a autenticação `SingleOrg` ou `IndividualB2C`. O valor padrão é `/signin-oidc`.

`-r|--org-read-access` – Permite que esse aplicativo tenha acesso de leitura ao diretório. Aplicável apenas à autenticação `SingleOrg` ou `MultiOrg`.

`--use-launch-settings` – Inclui *launchSettings.json* na saída do modelo gerado.

`--use-browserlink` – Inclui BrowserLink no projeto.

`-uld|--use-local-db` – Especifica que LocalDB deve ser usado em vez de SQLite. Aplicável apenas à autenticação `Individual` ou `IndividualB2C`.

`--no-restore` – Não executa uma restauração implícita durante a criação do projeto.

**page**

`-na|--namespace <NAMESPACE_NAME>` – Namespace do código gerado. O valor padrão é `MyApp.Namespace`.

`-np|--no-pagemodel` – Cria a página sem um PageModel.

**viewimports**

`-na|--namespace <NAMESPACE_NAME>` – Namespace do código gerado. O valor padrão é `MyApp.Namespace`.

# <a name="net-core-1xtabnetcore1x"></a>[.NET Core 1.x](#tab/netcore1x)

**console, xunit, mstest, web, webapi**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp1.0` ou `netcoreapp1.1`. O valor padrão é `netcoreapp1.0`.

**classlib**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp1.0`, `netcoreapp1.1` ou `netstandard1.0` como `netstandard1.6`. O valor padrão é `netstandard1.4`.

**mvc**

`-f|--framework <FRAMEWORK>` – especifica a qual [estrutura](../../standard/frameworks.md) se destina. Valores: `netcoreapp1.0` ou `netcoreapp1.1`. O valor padrão é `netcoreapp1.0`.

`-au|--auth <AUTHENTICATION_TYPE>` – O tipo de autenticação usado. Valores: `None` ou `Individual`. O valor padrão é `None`.

`-uld|--use-local-db` – Especifica se deve usar ou não o LocalDB em vez do SQLite. Valores: `true` ou `false`. O valor padrão é `false`.

---

## <a name="examples"></a>Exemplos

Crie um projeto de aplicativo de console em C# especificando o nome do modelo:

`dotnet new "Console Application"`

Crie um projeto de aplicativo de console F# no diretório atual:

`dotnet new console -lang F#`

Crie um projeto de biblioteca de classes .NET Standard no diretório especificado (disponível somente no SDK do .NET Core 2.0 ou em versões posteriores):

`dotnet new classlib -lang VB -o MyLibrary`

Crie um projeto de MVC em C# do ASP.NET Core no diretório atual sem autenticação:

`dotnet new mvc -au None`

Crie um projeto de xUnit:

`dotnet new xunit`

Lista todos os modelos disponíveis para o MVC:

`dotnet new mvc -l`

Liste todos os modelos que correspondem à substring *we*. Nenhuma correspondência exata é encontrada, portanto, a correspondência de substring é executada em relação tanto ao nome curto quanto às colunas de nome.

`dotnet new we -l`

Tentativa de invocar o modelo correspondente a *ng*. Se não for possível determinar uma única correspondência, liste os modelos que são correspondências parciais.

`dotnet new ng`

Instale a versão 2.0 dos modelos de aplicativo de página única do ASP.NET Core (opção de comando disponível somente para o SDK do .NET Core 1.1 e versões posteriores):

`dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0`

Crie um *global.json* no diretório atual definindo a versão do SDK como 2.0.0 (disponível somente no SDK do .NET Core 2.0 ou em versões posteriores):

`dotnet new globaljson --sdk-version 2.0.0`

## <a name="see-also"></a>Consulte também

- [Modelos personalizados para dotnet new](custom-templates.md)
- [Create a custom template for dotnet new](../tutorials/create-custom-template.md) (Criar um modelo personalizado para dotnet new)
- [Repositório do GitHub de dotnet/dotnet-template-samples](https://github.com/dotnet/dotnet-template-samples)
- [Modelos disponíveis para dotnet new](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)
