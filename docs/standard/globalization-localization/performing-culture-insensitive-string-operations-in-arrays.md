---
title: Executando operações de cadeia de caracteres que não levam em conta a cultura em matrizes
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- culture-insensitive string operations, in arrays
- arrays [.NET Framework], culture-insensitive string operations
- comparer parameter
ms.assetid: f12922e1-6234-4165-8896-63f0653ab478
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b52ff8d72132c1076dfdece5e1ce47bbece37b81
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70856001"
---
# <a name="performing-culture-insensitive-string-operations-in-arrays"></a>Executando operações de cadeia de caracteres que não levam em conta a cultura em matrizes

As sobrecargas dos métodos <xref:System.Array.Sort%2A?displayProperty=nameWithType> e <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> executam classificações que diferenciam a cultura por padrão usando a propriedade <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType>. Os resultados com diferenciação de cultura retornados por esses métodos podem variar com a cultura devido a diferenças em ordens de classificação. Para eliminar o comportamento que diferencia a cultura, use uma das sobrecargas do método que aceita um parâmetro `comparer`. O parâmetro `comparer` especifica a implementação <xref:System.Collections.IComparer> a ser usada ao comparar os elementos na matriz. Para o parâmetro, especifique uma classe de comparador invariável personalizado que usa <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>. Um exemplo de uma classe personalizada de comparador invariável é fornecido no subtópico "Usando a classe SortedList" do tópico [Executando operações de cadeia de caracteres de cultura em coleções](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-collections.md).

> [!NOTE]
> Passar **CultureInfo. InvariantCulture** para um método de comparação executa uma comparação que não diferencia a cultura. No entanto, não causa uma comparação não linguística, por exemplo, para caminhos de arquivos, chaves do Registro e variáveis de ambiente. Também não oferece suporte a decisões de segurança com base no resultado da comparação. Para obter uma comparação não linguística ou suporte para decisões de segurança com base no resultado, o aplicativo deve usar um método de comparação que aceite um valor <xref:System.StringComparison>. Assim, o aplicativo deve passar <xref:System.StringComparison.Ordinal>.

## <a name="see-also"></a>Consulte também

- <xref:System.Array.Sort%2A?displayProperty=nameWithType>
- <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType>
- <xref:System.Collections.IComparer>
- [Executando operações de cadeia de caracteres que não levam em conta a cultura](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)
