---
title: Palavra-chave namespace – Referência de C#
ms.custom: seoapril2019
ms.date: 07/20/2015
f1_keywords:
- namespace_CSharpKeyword
- namespace
helpviewer_keywords:
- namespace keyword [C#]
- scope [C#]
ms.assetid: 0a788423-9110-42e0-97d9-bda41ca4870f
ms.openlocfilehash: 8cc1d1461a33ab94f8ae399d6ff40f26eaf7f74a
ms.sourcegitcommit: cf9515122fce716bcfb6618ba366e39b5a2eb81e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69039458"
---
# <a name="namespace-c-reference"></a>namespace (Referência de C#)

A palavra-chave `namespace` é usada para declarar um escopo que contém um conjunto de objetos relacionados. Você pode usar um namespace para organizar elementos de código e criar tipos globalmente exclusivos.

[!code-csharp[csrefKeywordsNamespace#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#1)]

## <a name="remarks"></a>Comentários

Dentro de um namespace, é possível declarar zero ou mais dos seguintes tipos:

- outro namespace

- [class](class.md)

- [interface](interface.md)

- [struct](struct.md)

- [enum](enum.md)

- [delegate](delegate.md)

Quer você declare explicitamente ou não um namespace em um arquivo de origem C#, o compilador adiciona um namespace padrão. Este namespace sem nome, às vezes chamado de namespace global, está presente em todos os arquivos. Qualquer identificador no namespace global está disponível para uso em um namespace nomeado.

Os namespaces implicitamente têm acesso público e não isso é modificável. Para uma discussão sobre os modificadores de acesso que você pode atribuir a elementos em um namespace, consulte [Modificadores de acesso](access-modifiers.md).

É possível definir um namespace em duas ou mais declarações. Por exemplo, o exemplo a seguir define duas classes como parte do namespace `MyCompany`:

[!code-csharp[csrefKeywordsNamespace#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#2)]

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como chamar um método estático em um namespace aninhado.

[!code-csharp[csrefKeywordsNamespace#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#3)]

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para saber mais, confira a seção [Namespaces](~/_csharplang/spec/namespaces.md) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Palavras-chave do C#](index.md)
- [using](using-directive.md)
- [using static](using-static.md)
- [Qualificador de alias de namespace `::`](../operators/namespace-alias-qualifier.md)
- [Namespaces](../../programming-guide/namespaces/index.md)
