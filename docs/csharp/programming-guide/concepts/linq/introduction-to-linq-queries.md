---
title: Introdução a consultas LINQ (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- deferred execution [LINQ]
- LINQ, queries
- LINQ, deferred execution
- queries [LINQ], about LINQ queries
ms.assetid: 37895c02-268c-41d5-be39-f7d936fa88a8
ms.openlocfilehash: 6188af0ffea699899212e4bcf20b7c19f68858b4
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69924340"
---
# <a name="introduction-to-linq-queries-c"></a>Introdução a consultas LINQ (C#)
Uma *consulta* é uma expressão que recupera dados de uma fonte de dados. As consultas normalmente são expressas em uma linguagem de consulta especializada. Diferentes linguagens foram desenvolvidas ao longo do tempo para os diversos tipos de fontes de dados, por exemplo, SQL para bancos de dados relacionais e o XQuery para XML. Portanto, os desenvolvedores precisaram aprender uma nova linguagem de consulta para cada tipo de fonte de dados ou formato de dados que eles tinham que oferecer suporte. O [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] simplifica essa situação ao oferecer um modelo consistente para trabalhar com os dados entre vários tipos de fontes e formatos de dados. Em uma consulta [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], você está sempre trabalhando com objetos. Você usa os mesmos padrões básicos de codificação para consultar e transformar dados em documentos XML, bancos de dados SQL, conjuntos de dados do ADO.NET coleções do .NET e qualquer outro formato para o qual um provedor [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] estiver disponível.  
  
## <a name="three-parts-of-a-query-operation"></a>Três Partes de uma Operação de Consulta  
 Todos as operações de consulta [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] consistem em três ações distintas:  
  
1. Obter a fonte de dados.  
  
2. Criar a consulta.  
  
3. Executar a consulta.  
  
 O exemplo a seguir mostra como as três partes de uma operação de consulta são expressas em código-fonte. O exemplo usa uma matriz de inteiros como uma fonte de dados para sua conveniência. No entanto, os mesmos conceitos também se aplicam a outras fontes de dados. Faremos referência a este exemplo todo o restante deste tópico.  
  
 [!code-csharp[CsLINQGettingStarted#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#1)]  
  
 A ilustração a seguir mostra a operação de consulta completa. No [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], a execução da consulta é diferente da própria consulta. Em outras palavras, você não recupera dados apenas criando uma variável de consulta.  
  
 ![Diagrama da operação completa de consulta LINQ.](./media/introduction-to-linq-queries/linq-query-complete-operation.png)  
  
## <a name="the-data-source"></a>A Fonte de Dados  
 No exemplo anterior, como a fonte de dados é uma matriz, ela dá suporte à interface genérica <xref:System.Collections.Generic.IEnumerable%601> de forma implícita. Isso significa que ela pode ser consultada com o [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Uma consulta é executada em uma instrução `foreach`, e `foreach` requer <xref:System.Collections.IEnumerable> ou <xref:System.Collections.Generic.IEnumerable%601>. Tipos que dão suporte a <xref:System.Collections.Generic.IEnumerable%601> ou uma interface derivada, como a genérica <xref:System.Linq.IQueryable%601>, são chamados *tipos passíveis de consulta*.  
  
 Um tipo passível de consulta não exige modificação ou tratamento especial para servir como uma fonte de dados do [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Se os dados de origem ainda não estiverem na memória como um tipo passível de consulta, o provedor do [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] deverá representá-los como tal. Por exemplo, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] carrega um documento XML em um tipo <xref:System.Xml.Linq.XElement> passível de consulta:  
  
 [!code-csharp[CsLINQGettingStarted#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#2)]  
  
 Com o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)], primeiro você cria um mapeamento relacional de objeto em tempo de design, manualmente ou usando as [Ferramentas LINQ to SQL no Visual Studio](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2). Você escreve suas consultas aos objetos e o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] manipula a comunicação com o banco de dados em tempo de execução. No exemplo a seguir, `Customers` representa uma tabela específica no banco de dados, e o tipo do resultado da consulta, <xref:System.Linq.IQueryable%601>, deriva de <xref:System.Collections.Generic.IEnumerable%601>.  
  
```csharp  
Northwnd db = new Northwnd(@"c:\northwnd.mdf");  
  
// Query for customers in London.  
IQueryable<Customer> custQuery =  
    from cust in db.Customers  
    where cust.City == "London"  
    select cust;  
```  
  
 Para obter mais informações sobre como criar tipos específicos de fontes de dados, consulte a documentação para os diversos provedores do [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. No entanto, a regra básica é muito simples: uma fonte de dados [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] é qualquer objeto que dá suporte à interface genérica <xref:System.Collections.Generic.IEnumerable%601> ou uma interface que herda dela.  
  
> [!NOTE]
> Tipos, como <xref:System.Collections.ArrayList>, que dão suporte à interface <xref:System.Collections.IEnumerable> não genérica também podem ser usados como uma fonte de dados [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Para obter mais informações, confira [Como: Consultar uma ArrayList com LINQ (C#)](./how-to-query-an-arraylist-with-linq.md).  
  
## <a name="query"></a> A consulta  
 A consulta especifica quais informações devem ser recuperadas da fonte (ou fontes) de dados. Opcionalmente, uma consulta também especifica como essas informações devem ser classificadas, agrupadas e moldadas antes de serem retornadas. Uma consulta é armazenada em uma variável de consulta e é inicializada com uma expressão de consulta. Para tornar mais fácil escrever consultas, o C# introduziu uma nova sintaxe de consulta.  
  
 A consulta no exemplo anterior retorna todos os números pares da matriz de inteiros. A expressão de consulta contém três cláusulas: `from`, `where` e `select`. (Se você estiver familiarizado com o SQL, deve ter percebido que a ordem das cláusulas é invertida em relação à ordem no SQL). A cláusula `from` especifica a fonte de dados, a cláusula `where` aplica o filtro e a cláusula `select` especifica o tipo dos elementos retornados. Essas e outras cláusulas de consulta são discutidas detalhadamente na seção [Expressões de consulta LINQ](../../linq-query-expressions/index.md). Por enquanto, o ponto importante é que, no [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], a variável de consulta não faz nada e não retorna nenhum dado. Ele apenas armazena as informações necessárias para produzir os resultados quando a consulta for executada em um momento posterior. Para obter mais informações sobre como as consultas são construídas nos bastidores, consulte [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md).  
  
> [!NOTE]
> As consultas também podem ser expressas usando a sintaxe de método. Para obter mais informações, consulte [Sintaxe de consulta e sintaxe de método em LINQ](./query-syntax-and-method-syntax-in-linq.md).  
  
## <a name="query-execution"></a>Execução da Consulta  
  
### <a name="deferred-execution"></a>Execução Adiada  
 Conforme mencionado anteriormente, a variável de consulta armazena somente os comandos da consulta. A execução real da consulta é adiada até que você itere sobre a variável de consulta em uma instrução `foreach`. Esse conceito é conhecido como *execução adiada* e é demonstrado no exemplo a seguir:  
  
 [!code-csharp[csLinqGettingStarted#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#4)]  
  
 A instrução `foreach` também é o local em que os resultados da consulta são recuperados. Por exemplo, na consulta anterior, a variável de iteração `num` armazena cada valor (um de cada vez) na sequência retornada.  
  
 Como a própria variável de consulta nunca armazena os resultados da consulta, você poderá executá-la quantas vezes desejar. Por exemplo, você pode ter um banco de dados que está sendo atualizado continuamente por um aplicativo separado. Em seu aplicativo, você poderia criar uma consulta que recupera os dados mais recentes e poderia executá-la repetidamente em algum intervalo para recuperar resultados diferentes a cada vez.  
  
### <a name="forcing-immediate-execution"></a>Forçando Execução Imediata  
 As consultas que realizam funções de agregação em um intervalo de elementos de origem devem primeiro iterar sobre esses elementos. Exemplos dessas consultas são `Count`, `Max`, `Average` e `First`. Essas consultas são executadas sem uma instrução `foreach` explícita porque a consulta em si deve usar `foreach` para retornar um resultado. Observe também que esses tipos de consultas retornam um valor único e não uma coleção `IEnumerable`. A consulta a seguir retorna uma contagem de números pares na matriz de origem:  
  
 [!code-csharp[csLinqGettingStarted#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#5)]  
  
 Para forçar a execução imediata de qualquer consulta e armazenar seus resultados em cache, você pode chamar os métodos <xref:System.Linq.Enumerable.ToList%2A> ou <xref:System.Linq.Enumerable.ToArray%2A>.  
  
 [!code-csharp[csLinqGettingStarted#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#6)]  
  
 Você também pode forçar a execução colocando o loop `foreach` imediatamente após a expressão de consulta. No entanto, ao chamar `ToList` ou `ToArray`, você também armazena em cache todos os dados em um único objeto de coleção.  
  
## <a name="see-also"></a>Consulte também

- [Introdução a LINQ em C#](./getting-started-with-linq.md)
- [Passo a passo: Escrevendo consultas em C#](./walkthrough-writing-queries-linq.md)
- [Expressões de consulta LINQ](../../linq-query-expressions/index.md)
- [foreach, in](../../../language-reference/keywords/foreach-in.md)
- [Palavras-chave de Consulta (LINQ)](../../../language-reference/keywords/query-keywords.md)
