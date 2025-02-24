---
title: NAVEGAR (Entity SQL)
ms.date: 03/30/2017
ms.assetid: f107f29d-005f-4e39-a898-17f163abb1d0
ms.openlocfilehash: 2c6c2ae4c593da1d5fe8cdf3015eb0e31e4b12b5
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70249946"
---
# <a name="navigate-entity-sql"></a>NAVEGAR (Entity SQL)

Navega sobre a relação entre estabelecida entidades.

## <a name="syntax"></a>Sintaxe

```
navigate(instance-expression, [relationship-type], [to-end [, from-end] ])
```

## <a name="arguments"></a>Arguments

`instance-expression`Uma instância de uma entidade.

`relationship-type`O nome do tipo da relação, do arquivo CSDL (linguagem de definição de esquema conceitual). O `relationship-type` é qualificado como \<namespace >.\< nome do tipo de relação >.

`to`O fim da relação.

`from`O início da relação.

## <a name="return-value"></a>Valor de retorno

Se a cardinalidade da finalizar é 1, o valor de retorno será `Ref<T>`. Se a cardinalidade da finalizar é, no valor de retorno será `Collection<Ref<T>>`.

## <a name="remarks"></a>Comentários

As relações são construções de primeira classe no Modelo de Dados de Entidade (EDM). As relações podem ser estabelecidas entre dois ou mais tipos de entidade, e os usuários podem navegar sobre a relação de fim (entidade) para outra. `from` e `to` condicional são opcionais quando não há nenhuma ambiguidade na resolução de nomes dentro da relação.

NAVIGATE é válido no espaço de C e de 2.0.

O formulário geral de uma compilação de navegação é o seguinte:

navegue (`instance-expression`, `relationship-type`, [ `to-end` , `from-end` ] [])

Por exemplo:

```sql
Select o.Id, navigate(o, OrderCustomer, Customer, Order)
From LOB.Orders as o
```

Onde OrderCustomer está `relationship`, e o cliente e ordem são `to-end` (cliente) e `from-end` (ordem) da relação. Se OrderCustomer era uma relação n:1, o tipo de resultado da expressão Navigate é Ref\<Customer >.

A forma mais simples desta expressão é o seguinte:

```sql
Select o.Id, navigate(o, OrderCustomer)
From LOB.Orders as o
```

Da mesma forma, em uma consulta do formulário a seguir, a expressão Navigate produzirá uma coleção\<< ordem de referência > >.

```sql
Select c.Id, navigate(c, OrderCustomer, Order, Customer)
From LOB.Customers as c
```

A instância expressão deve ser um tipo de entidade/referência.

## <a name="example"></a>Exemplo

A seguinte consulta SQL Entity usa o operador NAVEGAR IN para navegar sobre o relacionamento estabelecido entre tipos de entidade de endereço e de SalesOrderHeader. A consulta é baseada no modelo de vendas AdventureWorks. Para compilar e executar essa consulta, siga estas etapas:

1. Siga o procedimento em [como: Executar uma consulta que retorna resultados](../how-to-execute-a-query-that-returns-structuraltype-results.md)de estruturaistype.

2. Passe a consulta a seguir como um argumento para o método `ExecuteStructuralTypeQuery`:

 [!code-csharp[DP EntityServices Concepts 2#NAVIGATE](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#navigate)]

## <a name="see-also"></a>Consulte também

- [Referência de Entity SQL](entity-sql-reference.md)
- [Como: Navegar pelas relações com o operador de navegação](navigate-entity-sql.md)
