---
title: Tipo de dados decimal (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Decimal
helpviewer_keywords:
- literal type characters [Visual Basic], D
- trailing zeros
- real numbers
- trailing 0 characters [Visual Basic]
- Decimal data type
- D literal type character [Visual Basic]
- decimals, Decimal data type
- 0 characters [Visual Basic], trailing
- data types [Visual Basic], assigning
- decimal keyword [Visual Basic]
- numbers [Visual Basic], real
- variable-precision numbers
- zeros, trailing
- '@ identifier type character'
- identifier type characters [Visual Basic], @
ms.assetid: 1d855b45-afe2-45b0-a623-96b6f63a43d5
ms.openlocfilehash: bab5a0bd7e0a85d550362bc3c1166566f6dcb81b
ms.sourcegitcommit: 463f3f050cecc0b6403e67f19a61f870fb8e7b7d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68512781"
---
# <a name="decimal-data-type-visual-basic"></a>Tipo de dados decimal (Visual Basic)

Contém valores de 128 bits (16 bytes) assinados que representam os números de inteiro de 96 bits (12 bytes) dimensionados por uma potência variável de 10. O fator de dimensionamento especifica o número de dígitos à direita do ponto decimal; Ele varia de 0 a 28. Com uma escala de 0 (sem casas decimais), o maior valor possível é +/-79228162514264337593543950335 (+/-7.9228162514264337593543950335E + 28). Com 28 casas decimais, o maior valor é +/-7.9228162514264337593543950335 e o menor valor diferente de zero é +/-0, 1 (+/-1E-28).

## <a name="remarks"></a>Comentários

O `Decimal` tipo de dados fornece o maior número de dígitos significativos para um número. Ele dá suporte a até 29 dígitos significativos e pode representar valores acima de 7,9228 x 10 ^ 28. Ele é particularmente adequado para cálculos, como financeiro, que exigem um grande número de dígitos, mas não podem tolerar erros de arredondamento.

O valor padrão de `Decimal` é 0.

## <a name="programming-tips"></a>Dicas de programação

- **Preciso.** `Decimal`Não é um tipo de dados de ponto flutuante. A `Decimal` estrutura contém um valor inteiro binário, junto com um bit de sinal e um fator de dimensionamento inteiro que especifica qual parte do valor é uma fração decimal. Por isso, `Decimal` os números têm uma representação mais precisa na memória do que os tipos de ponto flutuante `Double`(`Single` e).

- **Desempenho.** O `Decimal` tipo de dados é o mais lento de todos os tipos numéricos. Você deve avaliar a importância da precisão em relação ao desempenho antes de escolher um tipo de dados.

- **Ampliação.** O `Decimal` tipo de dados amplia para `Single` ou `Double`. Isso significa que você pode `Decimal` converter para qualquer um desses tipos sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.

- **Zeros à direita.** Visual Basic não armazena zeros à direita em um `Decimal` literal. No entanto `Decimal` , uma variável preserva os zeros à direita adquiridos de computação. O exemplo a seguir ilustra essa situação.

  ```vb
  Dim d1, d2, d3, d4 As Decimal
  d1 = 2.375D
  d2 = 1.625D
  d3 = d1 + d2
  d4 = 4.000D
  MsgBox("d1 = " & CStr(d1) & ", d2 = " & CStr(d2) &
        ", d3 = " & CStr(d3) & ", d4 = " & CStr(d4))
  ```

  A saída do `MsgBox` no exemplo anterior é a seguinte:

  ```
  d1 = 2.375, d2 = 1.625, d3 = 4.000, d4 = 4
  ```

- **Digite os caracteres.** Acrescentar o caractere de tipo literal `D` a um literal o força ao tipo de dados `Decimal`. Acrescentar o caractere de tipo identificador `@` a qualquer identificador o força ao tipo `Decimal`.

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.Decimal?displayProperty=nameWithType>.

## <a name="range"></a>Intervalo
 Talvez seja necessário usar o caractere `D` de tipo para atribuir um valor grande a uma `Decimal` variável ou constante. Esse requisito é porque o compilador interpreta um literal como `Long` a menos que um caractere de tipo literal siga o literal, como mostra o exemplo a seguir.

```vb
Dim bigDec1 As Decimal = 9223372036854775807   ' No overflow.
Dim bigDec2 As Decimal = 9223372036854775808   ' Overflow.
Dim bigDec3 As Decimal = 9223372036854775808D  ' No overflow.
```

A declaração para `bigDec1` não produz um estouro porque o valor atribuído a ele está dentro do intervalo de. `Long` O `Long` valor pode ser atribuído `Decimal` à variável.

A declaração para `bigDec2` gera um erro de estouro porque o valor atribuído a ele é muito grande para. `Long` Como o literal numérico não pode ser interpretado `Long`primeiro como um, ele não pode `Decimal` ser atribuído à variável.

Para `bigDec3`, o caractere `D` de tipo literal resolve o problema forçando o compilador a interpretar o literal como um `Decimal` , em vez de `Long`como um.

## <a name="see-also"></a>Consulte também

- <xref:System.Decimal?displayProperty=nameWithType>
- <xref:System.Decimal.%23ctor%2A?displayProperty=nameWithType>
- <xref:System.Math.Round%2A?displayProperty=nameWithType>
- [Tipos de Dados](../../../visual-basic/language-reference/data-types/index.md)
- [Tipo de Dados Simples](../../../visual-basic/language-reference/data-types/single-data-type.md)
- [Tipo de Dados Duplo](../../../visual-basic/language-reference/data-types/double-data-type.md)
- [Funções de Conversão do Tipo](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [Resumo da Conversão](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [Uso Eficiente de Tipos de Dados](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
