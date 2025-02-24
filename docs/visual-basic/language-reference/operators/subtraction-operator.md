---
title: '- Operador (Visual Basic)'
ms.date: 07/20/2015
f1_keywords:
- vb.Negate
- vb.-
helpviewer_keywords:
- operator [Visual Basic]
- operators [Visual Basic], subtraction
- operators [Visual Basic], difference
- negation operator [Visual Basic]
- operators [Visual Basic], arithmetic
- subtraction operator [Visual Basic], syntax
- arithmetic operators [Visual Basic], subtraction
- difference operator [Visual Basic]
- math operators [Visual Basic]
- operators [Visual Basic], negation
- minus operator [Visual Basic]
ms.assetid: bff2c368-662d-4c92-ac87-1d9bdfd3426a
ms.openlocfilehash: eb34b34986613f36b624c43c04f98390ffba4fe0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69965861"
---
# <a name="--operator-visual-basic"></a>Operador - (Visual Basic)
Retorna a diferença entre duas expressões numéricas ou o valor negativo de uma expressão numérica.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
      expression1 – expression2  
- or -  
– expression1  
```  
  
## <a name="parts"></a>Partes  
 `expression1`  
 Necessário. Qualquer expressão numérica.  
  
 `expression2`  
 Necessário, a `–` menos que o operador esteja calculando um valor negativo. Qualquer expressão numérica.  
  
## <a name="result"></a>Resultado  
 O resultado é a diferença entre `expression1` e `expression2`, ou o valor negado de `expression1`.  
  
 O tipo de dados de resultado é um tipo numérico apropriado para os tipos `expression1` de `expression2`dados de e. Consulte as tabelas "aritmética de inteiros" em [tipos de dados de resultados do operador](../../../visual-basic/language-reference/operators/data-types-of-operator-results.md).  
  
## <a name="supported-types"></a>Tipos com suporte  
 Todos os tipos numéricos. Isso inclui os tipos de ponto flutuante e não assinados e `Decimal`.  
  
## <a name="remarks"></a>Comentários  
 No primeiro uso mostrado na sintaxe mostrada anteriormente, o `–` operador é o operador de subtração aritmética *binária* para a diferença entre duas expressões numéricas.  
  
 No segundo uso mostrado na sintaxe mostrada anteriormente, o `–` operador é o operador de negação unário para o valor negativo de uma expressão. Nesse sentido, a negação consiste em reverter o sinal de `expression1` para que o resultado seja positivo se `expression1` for negativo.  
  
 Se qualquer expressão for avaliada como [Nothing](../../../visual-basic/language-reference/nothing.md), `–` o operador a tratará como zero.  
  
> [!NOTE]
> O `–` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de que você entendeu seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa `–` o operador para calcular e retornar a diferença entre dois números e, em seguida, para negar um número.  
  
 [!code-vb[VbVbalrOperators#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#10)]  
  
 Após a execução dessas instruções, `binaryResult` contém 124,45 e `unaryResult` contém – 334,90.  
  
## <a name="see-also"></a>Consulte também

- [-= Operador (Visual Basic)](../../../visual-basic/language-reference/operators/subtraction-assignment-operator.md)
- [Operadores Aritméticos](../../../visual-basic/language-reference/operators/arithmetic-operators.md)
- [Precedência do operador no Visual Basic](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [Operadores Listados por Funcionalidade](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [Operadores aritméticos no Visual Basic](../../../visual-basic/programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
