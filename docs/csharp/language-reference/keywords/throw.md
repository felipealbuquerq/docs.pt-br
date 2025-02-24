---
title: throw – Referência de C#
ms.custom: seodec18
ms.date: 03/02/2015
f1_keywords:
- throw
- throw_CSharpKeyword
helpviewer_keywords:
- throw statement [C#]
- throw expression [C#]
- throw keyword [C#]
ms.assetid: 5ac4feef-4b1a-4c61-aeb4-61d549e5dd42
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 4a9a235da06427e12bb5866a48f76f9c5896a572
ms.sourcegitcommit: 1b020356e421a9314dd525539da12463d980ce7a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70168740"
---
# <a name="throw-c-reference"></a>throw (Referência de C#)

Indica a ocorrência de uma exceção durante a execução do programa.  
  
## <a name="remarks"></a>Comentários

A sintaxe de `throw` é:

```csharp
throw [e]
```

em que `e` é uma instância de uma classe derivada de <xref:System.Exception?displayProperty=nameWithType>. O exemplo a seguir usará a instrução `throw` para gerar um <xref:System.IndexOutOfRangeException> se o argumento passado para um método chamado `GetNumber` não corresponder a um índice válido de uma matriz interna.

[!code-csharp[csrefKeyword#1](~/samples/snippets/csharp/language-reference/keywords/throw/throw-1.cs#1)]  

Os chamadores do método, então, usam um bloco `try-catch` ou `try-catch-finally` para tratar a exceção gerada. O exemplo a seguir trata a exceção gerada pelo método `GetNumber`.

[!code-csharp[csrefKeyword#2](~/samples/snippets/csharp/language-reference/keywords/throw/throw-1.cs#2)]  

## <a name="re-throwing-an-exception"></a>Gerando uma exceção novamente

`throw` também pode ser usado em um bloco `catch` para gerar novamente uma exceção tratada em um bloco `catch`.  Nesse caso, `throw` não usa um operando de exceção. Ele é mais útil quando um método passa um argumento de um chamador para algum outro método de biblioteca e o método de biblioteca gera uma exceção que deve ser passada para o chamador. O exemplo a seguir gera novamente um <xref:System.NullReferenceException> que é gerado ao tentar recuperar o primeiro caractere de uma cadeia de caracteres não inicializada.

[!code-csharp[csrefKeyword#3](~/samples/snippets/csharp/language-reference/keywords/throw/throw-3.cs#3)]  

> [!IMPORTANT]
> Também é possível usar a sintaxe `throw e` em um bloco `catch` para instanciar uma nova exceção que você passa para o chamador. Nesse caso, o rastreamento de pilha da exceção original, que fica disponível na propriedade <xref:System.Exception.StackTrace>, não é preservado.

## <a name="the-throw-expression"></a>A expressão `throw`

Começando com o C# 7.0, o `throw` pode ser usado como uma expressão, bem como uma instrução. Isso permite que uma exceção seja gerada em contextos que não tinham suporte anteriormente. Elas incluem:

- [o operador condicional](../operators/conditional-operator.md). O exemplo a seguir usará uma expressão `throw` para gerar um <xref:System.ArgumentException> se uma matriz de cadeia de caracteres vazia for passada para um método. Antes do C# 7.0, essa lógica precisava aparecer em uma instrução `if`/`else`.

   [!code-csharp[csrefKeyword#4](~/samples/snippets/csharp/language-reference/keywords/throw/conditional.cs#1)]  
  
- [o operador de união nula](../operators/null-coalescing-operator.md). No exemplo a seguir, uma expressão `throw` será usada com um operador de união nula para gerar uma exceção se a cadeia de caracteres atribuída a uma propriedade `Name` for `null`.

   [!code-csharp[csrefKeyword#5](~/samples/snippets/csharp/language-reference/keywords/throw/coalescing.cs#1)]  

- um método ou [lambda](../../programming-guide/statements-expressions-operators/lambda-expressions.md) com corpo de expressão. O exemplo a seguir ilustra um método com corpo de expressão que gera um <xref:System.InvalidCastException> porque uma conversão para um valor <xref:System.DateTime> não tem suporte.

   [!code-csharp[csrefKeyword#6](~/samples/snippets/csharp/language-reference/keywords/throw/exp-bodied.cs#1)]  

## <a name="c-language-specification"></a>Especificação da linguagem C#  

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Guia de Programação em C#](../../programming-guide/index.md)
- [try-catch](try-catch.md)
- [Palavras-chave do C#](index.md)
- [Como: Gerar exceções explicitamente](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
