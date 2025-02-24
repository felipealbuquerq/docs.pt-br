---
title: SELECIONAR (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 9a33bd0d-ded1-41e7-ba3c-305502755e3b
ms.openlocfilehash: 3d3564c37d8971d3261cb47acb774bd1b9f92192
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70249208"
---
# <a name="select-entity-sql"></a>SELECIONAR (Entity SQL)
Especifica os elementos retornados por uma consulta.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
SELECT [ ALL | DISTINCT ] [ topSubclause ] aliasedExpr   
      [{ , aliasedExpr }] FROM fromClause [ WHERE whereClause ] [ GROUP BY groupByClause [ HAVING havingClause ] ] [ ORDER BY orderByClause ]  
or  
SELECT VALUE [ ALL | DISTINCT ] [ topSubclause ] expr FROM fromClause [ WHERE whereClause ] [ GROUP BY groupByClause [ HAVING havingClause ] ] [ ORDER BY orderByClause  
```  
  
## <a name="arguments"></a>Arguments  
 ALL  
 Especifica que as duplicatas podem aparecer no conjunto de resultados. ALL é o padrão.  
  
 DISTINCT  
 Especifica que apenas resultados exclusivos podem aparecer no conjunto de resultados.  
  
 VALUE  
 Permite que somente um item seja especificado, e não adiciona um wrapper de linha.  
  
 `topSubclause`  
 Qualquer expressão válida que indica o número de primeiros resultados a serem retornados da consulta, do formulário `top(expr)`.  
  
 O parâmetro LIMIT do operador [order by](order-by-entity-sql.md) também permite que você selecione os primeiros n itens no conjunto de resultados.  
  
 `aliasedExpr`  
 Uma expressão da forma:  
  
 `expr`como `identifier` &#124;`expr`  
  
 `expr`  
 Um literal ou uma expressão.  
  
## <a name="remarks"></a>Comentários  
 A cláusula SELECT é avaliada após a avaliação das cláusulas [from](from-entity-sql.md), [Group by](group-by-entity-sql.md)e [having](having-entity-sql.md) . A cláusula SELECT pode se referir apenas aos itens atualmente no escopo (da cláusula FROM ou de escopos externos). Se uma cláusula GROUP BY tiver sido especificada, a cláusula SELECT estará autorizada apenas a referenciar os aliases das chaves GROUP BY. A referência aos itens da cláusula FROM é permitida somente em funções de agregação.  
  
 A lista de uma ou mais expressões de consulta após a palavra-chave SELECT é conhecida como a lista de seleção, ou mais formalmente como a projeção. A forma mais geral de projeção é uma única expressão de consulta. Se você selecionar um membro `member1` de uma coleção `collection1`, produzirá uma nova coleção de todos os `member1` valores de cada objeto no `collection1`, conforme ilustrado no exemplo a seguir.  
  
```  
SELECT collection1.member1 FROM collection1  
```  
  
 Por exemplo, se `customers` for uma coleção do tipo `Customer` que tem uma propriedade `Name` que é do tipo `string`, a `Name` seleção `customers` de gerará uma coleção de cadeias de caracteres, conforme ilustrado no exemplo a seguir .  
  
```  
SELECT customers.Name FROM customers AS c  
```  
  
 Também é possível usar a sintaxe JOIN (FULL, INNER, LEFT, OUTER, ON e RIGHT). ON é necessário para junções internas e não é permitido para junções cruzadas.  
  
## <a name="row-and-value-select-clauses"></a>Cláusulas de seleção de linha e valor  
 O [!INCLUDE[esql](../../../../../../includes/esql-md.md)] oferece suporte a duas variantes da cláusula SELECT. A primeira variante, seleção de linha, é identificada pela palavra-chave SELECT e pode ser usada para especificar um ou mais valores que devem ser projetados. Como um wrapper de linha é implicitamente adicionado ao redor dos valores retornados, o resultado da expressão de consulta é sempre um multiconjunto de linhas.  
  
 Cada expressão de consulta em uma seleção de linha deve especificar um alias. Se nenhum alias for especificado,[!INCLUDE[esql](../../../../../../includes/esql-md.md)] o tentará gerar um alias usando as regras de geração de alias.  
  
 Outra variante da cláusula SELECT, seleção de valor, é identificada pela palavra-chave SELECT VALUE. Ela permite que somente um valor seja especificado, e não adiciona um wrapper de linha.  
  
 Uma seleção de linha sempre pode ser expressa em termos de VALUE SELECT, conforme ilustrado no exemplo a seguir.  
  
```  
SELECT 1 AS a, "abc" AS b FROM C  
SELECT VALUE ROW(1 AS a, "abc" AS b) FROM C   
```  
  
## <a name="all-and-distinct-modifiers"></a>Modificadores All e Distinct  
 Ambas as variantes de SELECT no [!INCLUDE[esql](../../../../../../includes/esql-md.md)] permitem a especificação de um modificador ALL ou DISTINCT. Se o modificador DISTINCT for especificado, as duplicatas serão eliminadas da coleção gerada pela expressão de consulta (até, e inclusive, a cláusula SELECT). Se o modificador ALL for especificado, nenhuma eliminação de duplicada será executada; ALL é o padrão.  
  
## <a name="differences-from-transact-sql"></a>Diferenças de Transact-SQL  
 Diferentemente do Transact-SQL, o [!INCLUDE[esql](../../../../../../includes/esql-md.md)] não oferece suporte ao uso do argumento * na cláusula SELECT.  Em vez disso, o [!INCLUDE[esql](../../../../../../includes/esql-md.md)] permite que as consultas projetem registros inteiros referenciando aliases de coleção da cláusula FROM, conforme ilustrado no exemplo a seguir.  
  
```  
SELECT * FROM T1, T2  
```  
  
 A expressão de consulta Transact-SQL anterior é expressa [!INCLUDE[esql](../../../../../../includes/esql-md.md)] da seguinte maneira.  
  
```  
SELECT a1, a2 FROM T1 AS a1, T2 AS a2  
```  
  
## <a name="example"></a>Exemplo  
 A consulta Entity SQL a seguir usa o operador SELECT para especificar elementos a serem retornados por uma consulta. A consulta é baseada no modelo de vendas AdventureWorks. Para compilar e executar essa consulta, siga estas etapas:  
  
1. Siga o procedimento em [como: Executar uma consulta que retorna resultados](../how-to-execute-a-query-that-returns-structuraltype-results.md)de estruturaistype.  
  
2. Passe a consulta a seguir como um argumento para o método `ExecuteStructuralTypeQuery`:  
  
 [!code-csharp[DP EntityServices Concepts 2#LESS](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#less)]  
  
## <a name="see-also"></a>Consulte também

- [Expressões de Consulta](query-expressions-entity-sql.md)
- [Referência de Entity SQL](entity-sql-reference.md)
- [TOP](top-entity-sql.md)
