---
title: 'Como: usar vários tokens de segurança do mesmo tipo'
ms.date: 03/30/2017
ms.assetid: cf179f48-4ed4-4caa-86a5-ef8eecc231cd
ms.openlocfilehash: 1b383c6ccd96d1b3d7b091b2d7c67bb166da51df
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65589424"
---
# <a name="how-to-use-multiple-security-tokens-of-the-same-type"></a>Como: usar vários tokens de segurança do mesmo tipo
- No .NET Framework 3.0, uma mensagem de cliente continha somente um token de qualquer tipo. Agora, as mensagens do cliente podem conter vários tokens de um tipo. Este tópico mostra como incluir vários tokens do mesmo tipo em uma mensagem do cliente.  
  
- Observe que você não pode configurar um serviço dessa maneira: um serviço pode conter apenas um token de suporte.  
  
### <a name="to-use-multiple-security-tokens-of-the-same-type"></a>Para usar vários tokens de segurança do mesmo tipo  
  
1. Crie uma coleção de elementos de associação vazia a ser preenchido.  
  
     [!code-csharp[C_CustomBinding#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#9)]  
  
2. Criar uma <xref:System.ServiceModel.Channels.SecurityBindingElement> chamando <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A>.  
  
     [!code-csharp[C_CustomBinding#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#10)]  
  
3. Criar um <xref:System.ServiceModel.Security.Tokens.SupportingTokenParameters> coleção.  
  
     [!code-csharp[C_CustomBinding#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#11)]  
  
4. Adicione tokens SAML à coleção.  
  
     [!code-csharp[C_CustomBinding#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#12)]  
  
5. Adicionar a coleção para a <xref:System.ServiceModel.Channels.SecurityBindingElement>.  
  
     [!code-csharp[C_CustomBinding#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#13)]  
  
6. Adicione elementos de associação para a coleção de elementos de associação.  
  
     [!code-csharp[C_CustomBinding#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#14)]  
  
7. Retorne uma nova associação personalizada criada da coleção do elemento de associação.  
  
     [!code-csharp[C_CustomBinding#15](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#15)]  
  
## <a name="example"></a>Exemplo  
 Este é todo o método descrito pelo procedimento anterior.  
  
 [!code-csharp[C_CustomBinding#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#7)]  
