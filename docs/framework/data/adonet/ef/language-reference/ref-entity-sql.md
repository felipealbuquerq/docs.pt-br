---
title: REF (Entity SQL)
ms.date: 03/30/2017
ms.assetid: c5f4cb35-69e9-44cc-b63b-ee38922bbda1
ms.openlocfilehash: 9d35306d1299e91ecaa55a7d2818ee1e2982793f
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70249192"
---
# <a name="ref-entity-sql"></a>REF (Entity SQL)
Retorna uma referência a uma instância de entidade.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
REF( expression )   
```  
  
## <a name="arguments"></a>Arguments  
 `expression`  
 Qualquer expressão válida que produzir uma instância de um tipo de objeto.  
  
## <a name="return-value"></a>Valor de retorno  
 Uma referência à instância de entidade especificada.  
  
## <a name="remarks"></a>Comentários  
 Uma referência de entidade consiste da chave de entidade e um nome de conjunto de entidades. Porque os conjuntos de entidades diferentes podem ser baseados no mesmo tipo de entidade, uma chave específica de entidade pode aparecer em vários conjuntos de entidades. No entanto, uma referência de entidade é sempre exclusivo. Se a expressão de entrada representa um objeto armazenado, uma referência a essa entidade será retornada. Se a expressão de entrada não é uma entidade armazenado, uma referência nula será retornada.  
  
 Se o operador de extração de propriedade (.) é usada para acessar uma propriedade de uma entidade, a referência é desreferenciada automaticamente.  
  
## <a name="example"></a>Exemplo  
 A seguinte consulta SQL Entity usa o operador de referência para retornar a referência para um argumento de entidade de entrada. A mesma consulta desreferencia a referência porque estamos usando uma operação de extração de propriedade (.) para acessar uma propriedade de entidade do produto. A consulta é baseada no modelo de vendas AdventureWorks. Para compilar e executar essa consulta, siga estas etapas:  
  
1. Siga o procedimento em [como: Executar uma consulta que retorna os resultados](../how-to-execute-a-query-that-returns-primitivetype-results.md)de primitivatype.  
  
2. Passe a consulta a seguir como um argumento para o método `ExecutePrimitiveTypeQuery`:  
  
 [!code-csharp[DP EntityServices Concepts 2#REF](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#ref)]  
  
## <a name="see-also"></a>Consulte também

- [DEREF](deref-entity-sql.md)
- [CREATEREF](createref-entity-sql.md)
- [KEY](key-entity-sql.md)
- [Referência de Entity SQL](entity-sql-reference.md)
- [Definições de tipo](type-definitions-entity-sql.md)
