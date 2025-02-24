---
title: Assegurando a integridade dos dados com códigos hash
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generating hash
- verifying hash codes
- cryptography [.NET Framework], hash
- data integrity
- checking hash codes
- encryption [.NET Framework], hash
- hash
ms.assetid: 33660f33-b70f-4dca-8c87-ab35cfc2961a
author: mairaw
ms.author: mairaw
ms.openlocfilehash: a0383dc3024352b9fac879532ab2789a60488c96
ms.sourcegitcommit: 09d699aca28ae9723399bbd9d3d44aa0cbd3848d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68331640"
---
# <a name="ensuring-data-integrity-with-hash-codes"></a>Assegurando a integridade dos dados com códigos hash
Um valor de hash é um valor numérico de um comprimento fixo que identifica dados exclusivamente. Os valores de hash representam grandes quantidades de dados como valores numéricos muito menores, para que sejam usados com assinaturas digitais. Você pode assinar um valor de hash com mais eficiência do que assinar o valor maior. Os valores de hash também são úteis para verificar a integridade dos dados enviados por meio de canais inseguros. O valor de hash dos dados recebidos pode ser comparado ao valor de hash dos dados conforme eles foram enviados para determinar se os dados foram alterados.  
  
 Este tópico descreve como gerar e verificar os códigos de hash usando as classes no <xref:System.Security.Cryptography?displayProperty=nameWithType> namespace.  
  
## <a name="generating-a-hash"></a>Gerando um hash  
 As classes de hash gerenciadas podem aplicar hash a uma matriz de bytes ou a um objeto de fluxo gerenciado. O exemplo a seguir usa o algoritmo de hash SHA1 para criar um valor de hash para uma cadeia de caracteres. O exemplo usa a <xref:System.Text.UnicodeEncoding> classe para converter a cadeia de caracteres em uma matriz de bytes com hash usando a <xref:System.Security.Cryptography.SHA1Managed> classe. O valor de hash é exibido para o console.  

 Devido a problemas de colisão com o SHA1, a Microsoft recomenda SHA256 ou melhor.
  
 [!code-csharp[GeneratingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/generatingahash/cs/program.cs#1)]
 [!code-vb[GeneratingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/generatingahash/vb/program.vb#1)]  
  
 Esse código exibirá a seguinte cadeia de caracteres para o console:  
  
 `59 4 248 102 77 97 142 201 210 12 224 93 25 41 100 197 213 134 130 135`  
  
## <a name="verifying-a-hash"></a>Verificando um hash  
 Os dados podem ser comparados a um valor de hash para determinar sua integridade. Normalmente, os dados são codificados em hash em um determinado momento e o valor de hash é protegido de alguma forma. Mais tarde, os dados podem ser codificados novamente e comparados com o valor protegido. Se os valores de hash forem correspondentes, os dados não foram alterados. Se os valores não corresponderem, os dados ficarão corrompidos. Para que esse sistema funcione, o hash protegido deve ser criptografado ou mantido em segredo de todas as partes não confiáveis.  
  
 O exemplo a seguir compara o valor de hash anterior de uma cadeia de caracteres com um novo valor de hash. Este exemplo percorre cada byte dos valores de hash e faz uma comparação.  
  
 [!code-csharp[VerifyingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/verifyingahash/cs/program.cs#1)]
 [!code-vb[VerifyingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/verifyingahash/vb/program.vb#1)]  
  
 Se os dois valores de hash corresponderem, esse código exibirá o seguinte no console:  
  
```  
The hash codes match.  
```  
  
 Se eles não corresponderem, o código exibirá o seguinte:  
  
```  
The hash codes do not match.  
```  
  
## <a name="see-also"></a>Consulte também

- [Serviços criptográficos](../../../docs/standard/security/cryptographic-services.md)
