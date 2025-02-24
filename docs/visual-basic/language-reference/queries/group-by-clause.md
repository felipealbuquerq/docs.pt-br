---
title: Cláusula Group By (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.QueryGroupByInto
- vb.QueryGroupBy
- vb.QueryGroupRef
- vb.QueryGroupInto
- vb.QueryGroup
helpviewer_keywords:
- queries [Visual Basic], Group By
- Group By statement [Visual Basic]
- Group By clause [Visual Basic]
ms.assetid: b1b5dcea-6654-473b-a2db-01f7e4c265d7
ms.openlocfilehash: 04378d2c9a7e565343ff663997e2a3e61f04f9d2
ms.sourcegitcommit: 10986410e59ff29f2ec55c6759bde3eb4d1a00cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66423584"
---
# <a name="group-by-clause-visual-basic"></a>Cláusula Group By (Visual Basic)
Agrupa os elementos de um resultado de consulta. Também pode ser usado para aplicar funções de agregação a cada grupo. A operação de agrupamento se baseia em uma ou mais chaves.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
Group [ listField1 [, listField2 [...] ] By keyExp1 [, keyExp2 [...] ]  
  Into aggregateList  
```  
  
## <a name="parts"></a>Partes  
  
- `listField1`, `listField2`  
  
     Opcional. Um ou mais campos de variável de consulta ou variáveis que identificam explicitamente os campos a serem incluídos no resultado agrupado. Se nenhum campo foi especificado, todos os campos de variável de consulta ou variáveis serão incluídos no resultado agrupado.  
  
- `keyExp1`  
  
     Necessário. Uma expressão que identifica a chave a ser usada para determinar os grupos de elementos. Você pode especificar mais de uma chave para especificar uma chave composta.  
  
- `keyExp2`  
  
     Opcional. Uma ou mais chaves adicionais que são combinadas com `keyExp1` para criar uma chave composta.  
  
- `aggregateList`  
  
     Necessário. Uma ou mais expressões que identificam como os grupos são agregados. Para identificar um nome de membro para os resultados agrupados, use o `Group` palavra-chave, que pode estar em qualquer uma das seguintes formas:  
  
    ```  
    Into Group  
    ```  
  
     - ou -  
  
    ```  
    Into <alias> = Group  
    ```  
  
     Você também pode incluir funções agregadas para aplicar ao grupo.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar o `Group By` cláusula para dividir os resultados de uma consulta em grupos. O agrupamento se baseia em uma chave ou uma chave composta que consiste em várias chaves. Elementos que estão associados com valores de chave de correspondência são incluídos no mesmo grupo.  
  
 Você usa o `aggregateList` parâmetro do `Into` cláusula e o `Group` palavra-chave para identificar o nome do membro que é usado para fazer referência ao grupo. Você também pode incluir funções agregadas no `Into` cláusula para computar os valores para os elementos agrupados. Para obter uma lista das funções de agregação padrão, consulte [cláusula Aggregate](../../../visual-basic/language-reference/queries/aggregate-clause.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir agrupa uma lista de clientes com base em sua localização (país/região) e fornece uma contagem dos clientes em cada grupo. Os resultados são ordenados pelo nome de país/região. Os resultados agrupados são ordenados pelo nome da cidade.  
  
 [!code-vb[VbSimpleQuerySamples#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#11)]  
  
## <a name="see-also"></a>Consulte também

- [Introdução ao LINQ no Visual Basic](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](../../../visual-basic/language-reference/queries/index.md)
- [Cláusula Select](../../../visual-basic/language-reference/queries/select-clause.md)
- [Cláusula From](../../../visual-basic/language-reference/queries/from-clause.md)
- [Cláusula Order By](../../../visual-basic/language-reference/queries/order-by-clause.md)
- [Cláusula Aggregate](../../../visual-basic/language-reference/queries/aggregate-clause.md)
- [Cláusula Group Join](../../../visual-basic/language-reference/queries/group-join-clause.md)
