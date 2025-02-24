---
title: SqlMetal.exe (ferramenta de geração de código)
ms.date: 03/30/2017
helpviewer_keywords:
- SQLMetal [LINQ to SQL]
- code generation tool
- SQLMetal.exe
- LINQ to SQL, serialization
- LINQ to SQL, DBML files
- LINQ to SQL, SQLMetal
ms.assetid: 819e5a96-7646-4fdb-b14b-fe31221b0614
ms.openlocfilehash: f435c93f68feb564aaca0f52842e567aa688ac64
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69938002"
---
# <a name="sqlmetalexe-code-generation-tool"></a>SqlMetal.exe (ferramenta de geração de código)
A ferramenta de linha de comando SqlMetal gera o código e o mapeamento para o componente [!INCLUDE[vbtecdlinq](../../../includes/vbtecdlinq-md.md)] do .NET Framework. Aplicando-se opções exibidas mais à frente neste tópico, é possível instruir SqlMetal para executar diversas ações diferentes, dentre as quais estão:  
  
- Em um banco de dados, gere atributos de código-fonte e mapeamento ou um arquivo de mapeamento.  
  
- Em um banco de dados, gera um arquivo .dbml (database markup language) intermediário para personalização.  
  
- Em um arquivo .dbml, gere atributos de código e mapeamento ou um arquivo de mapeamento.  
  
 Essa ferramenta é instalada automaticamente com o Visual Studio. Por padrão, o arquivo está localizado em `drive`:\Program Files\Microsoft SDKs\Windows\v`n.nn`\bin. Se não instalar o Visual Studio, você também poderá obter o arquivo SQLMetal baixando o [SDK do Windows](https://go.microsoft.com/fwlink/?LinkId=142225).  
  
> [!NOTE]
> Os desenvolvedores que usam o Visual Studio também podem usar o Object Relational Designer para gerar classes da entidade. A abordagem de linha de comando é bem dimensionada para bancos de dados grandes. Como SqlMetal é uma ferramenta de linha de comando, é possível usá-lo em um processo de compilação.  
  
 Para executar a ferramenta, use o Prompt de Comando do Desenvolvedor para Visual Studio (ou o Prompt de Comando do Visual Studio no Windows 7). Para obter mais informações, consulte [Prompts de Comando](../../../docs/framework/tools/developer-command-prompt-for-vs.md). No prompt de comando, digite o seguinte:  
  
## <a name="syntax"></a>Sintaxe  
  
```  
sqlmetal [options] [<input file>]  
```  
  
## <a name="options"></a>Opções  
 Para exibir a lista de opções mais atual, digite `sqlmetal /?` em um prompt de comando no local instalado.  
  
 **Opções de Conexão**  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/server:** *\<name>*|Especifica o nome do servidor do banco de dados.|  
|**/database:** *\<name>*|Especifica o catálogo do banco de dados no servidor.|  
|**/user:** *\<name>*|Especifica a ID do usuário de logon. Valor padrão: usar a autenticação do Windows.|  
|**/password:** *\<password>*|Especifica a senha de logon. Valor padrão: usar a autenticação do Windows.|  
|**/conn:** *\<connection string>*|Especifica a cadeia de conexão do banco de dados. Não pode ser usada com as opções **/server**, **/database**, **/user** ou **/password**.<br /><br /> Não inclua o nome do arquivo na cadeia de conexão. Em vez disso, adicione o nome do arquivo à linha de comando como o arquivo de entrada. Por exemplo, a seguinte linha especifica “c:\northwnd.mdf” como o arquivo de entrada: **sqlmetal /code:"c:\northwind.cs" /language:csharp "c:\northwnd.mdf"** .|  
|**/timeout:** *\<seconds>*|Especifica o valor de tempo limite em que SqlMetal acessa o banco de dados. Valor padrão: 0 (ou seja, sem tempo limite).|  
  
 **Opções de extração**  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/views**|Extrai exibições do banco de dados.|  
|**/functions**|Extrai funções do banco de dados.|  
|**/sprocs**|Extrai procedimentos armazenados.|  
  
 **Opções de saída**  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/dbml** *[:file]*|Envia saídas como .dbml. Não pode ser usada com a opção **/map**.|  
|**/code** *[:file]*|Envia saídas como código-fonte. Não pode ser usada com a opção **/dbml**.|  
|**/map** *[:file]*|Gera um arquivo de mapeamento XML, em vez de atributos. Não pode ser usada com a opção **/dbml**.|  
  
 **Diversos**  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**/language:** *\<language>*|Especifica a linguagem do código-fonte.<br /><br /> Valid *\<language>* : vb, csharp.<br /><br /> Valor padrão: derivado da extensão de nome de arquivo de código.|  
|**/namespace:** *\<name>*|Especifica o namespace do código gerado. Valor padrão: sem namespace.|  
|**/context:** *\<type>*|Especifica o nome da classe de contexto dos dados. Valor padrão: derivado do nome do banco de dados.|  
|**/entitybase:** *\<type>*|Especifica a classe base das classes de entidade no código gerado. Valor padrão: as entidades não têm classe base.|  
|**/pluralize**|Pluraliza ou singulariza automaticamente nomes de classe e de membro.<br /><br /> Essa opção só está disponível na versão em inglês  dos EUA.|  
|**/serialization:** *\<option>*|Gera classes serializáveis.<br /><br /> *\<opção>* válida: nenhum, unidirecional. Valor padrão: nenhuma.<br /><br /> Para obter mais informações, consulte [Serialização](../../../docs/framework/data/adonet/sql/linq/serialization.md).|  
  
 **Arquivo de Entrada**  
  
|Opção|DESCRIÇÃO|  
|------------|-----------------|  
|**\<input file>**|Especifica um arquivo .mdf do SQL Server Express, um arquivo .sdf do SQL Server Compact 3.5 ou um arquivo intermediário .dbml.|  
  
## <a name="remarks"></a>Comentários  
 A funcionalidade SqlMetal envolve, na realidade, duas etapas:  
  
- Extraindo os metadados do banco de dados para um arquivo. dbml.  
  
- Gerando um arquivo de saída do código.  
  
     Usando as opções da linha de comando apropriadas, é possível produzir o código-fonte do Visual Basic ou do C# ou produzir um arquivo de mapeamento de XML.  
  
 Para extrair os metadados de um arquivo .mdf, você deve especificar o nome do arquivo .mdf depois de todas as outras opções.  
  
 Se nenhum **/server** for especificado, **localhost/sqlexpress** será suposto.  
  
 O Microsoft SQL Server 2005 acionará uma exceção se uma ou mais das seguintes condições forem verdadeiras:  
  
- SqlMetal tenta extrair um procedimento armazenado que se chama.  
  
- O nível de aninhamento de um procedimento armazenado, função ou uma exibição excede 32.  
  
     SqlMetal captura essa exceção e a relata como um aviso.  
  
 Para especificar um nome do arquivo de entrada, adicione o nome do arquivo à linha de comando como o arquivo de entrada. Não há suporte para a inclusão do nome de arquivo na cadeia de conexão (usando a opção **/conn**).  
  
## <a name="examples"></a>Exemplos  
 Gera um arquivo. dbml que inclui metadados SQL extraídos:  
  
 **sqlmetal /server:myserver /database:northwind /dbml:mymeta.dbml**  
  
 Gera um arquivo. dbml que inclui metadados SQL extraídos de um arquivo .mdf usando-se o SQL Server Express:  
  
 **sqlmetal /dbml:mymeta.dbml mydbfile.mdf**  
  
 Gera um arquivo. dbml que inclui metadados SQL extraídos do SQL Server Express:  
  
 **sqlmetal /server:.\sqlexpress /dbml:mymeta.dbml /database:northwind**  
  
 Gera o código-fonte com base em um arquivo de metadados .dbml:  
  
 **sqlmetal /namespace:nwind /code:nwind.cs /language:csharp mymetal.dbml**  
  
 Gera o código-fonte com base nos metadados SQL diretamente:  
  
 **sqlmetal /server:myserver /database:northwind /namespace:nwind /code:nwind.cs /language:csharp**  
  
> [!NOTE]
> Ao usar a opção **/pluralize** com o banco de dados de exemplo Northwind, observe o comportamento a seguir. Quando SqlMetal cria nomes de tipo de linha para tabelas, os nomes de tabela são singulares. Quando ele cria propriedades <xref:System.Data.Linq.DataContext> para tabelas, os nomes de tabela serão plurais. Coincidentemente, as tabelas no banco de dados de exemplo Northwind já são plurais. Por isso, você não vê essa parte funcionando. Embora seja uma prática comum nomear tabelas de banco de dados singulares, também é uma prática comum no .NET nomear as coleções plurais.  
  
## <a name="see-also"></a>Consulte também

- [Como: gerar o modelo de objeto em Visual Basic ou em C#](../../../docs/framework/data/adonet/sql/linq/how-to-generate-the-object-model-in-visual-basic-or-csharp.md)
- [Geração de código em LINQ to SQL](../../../docs/framework/data/adonet/sql/linq/code-generation-in-linq-to-sql.md)
- [Mapeamento Externo](../../../docs/framework/data/adonet/sql/linq/external-mapping.md)
