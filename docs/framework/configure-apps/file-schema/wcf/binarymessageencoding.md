---
title: <binaryMessageEncoding>
ms.date: 03/30/2017
ms.assetid: e4ae3cd4-6b67-4ce1-af4b-9400e0a38dba
ms.openlocfilehash: feefd7fe73363b5fe1ec5658c5dc339c3d6bac57
ms.sourcegitcommit: 093571de904fc7979e85ef3c048547d0accb1d8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70398217"
---
# <a name="binarymessageencoding"></a>\<binaryMessageEncoding>
Define um codificador de mensagem binária que codificará mensagens Windows Communication Foundation (WCF) em binário na conexão.  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<> de System. serviceModel**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<associações >** ](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de CustomBinding**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> de associação**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> binaryMessageEncoding**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<binaryMessageEncoding maxReadPoolSize="Integer"
                       maxSessionSize="Integer"
                       maxWritePoolSize="Integer"
                       messageVersion="Soap11Addressing10/Soap12Addressing10" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|maxReadPoolSize|Um inteiro que define quantas mensagens podem ser lidas simultaneamente sem alocar novos leitores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 64.|  
|maxSessionSize|Um inteiro positivo que define o tamanho, em bytes, do buffer usado para codificação. Um buffer maior aumenta a velocidade de codificação às custas do tamanho do conjunto de trabalho. O padrão é 2048.|  
|maxWritePoolSize|Um inteiro que define quantas mensagens podem ser enviadas simultaneamente sem alocar novos gravadores. Tamanhos de pool maiores tornam o sistema mais tolerante a picos de atividade no custo de um conjunto de trabalho maior. O padrão é 16.|  
|messageVersion|Especifica a mensagem SOAP e as versões WS-Addressing que são usadas ou esperadas.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](../../../misc/binding.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 A codificação é o processo de transformar uma mensagem em uma sequência de bytes. A decodificação é o processo reverso. O Windows Communication Foundation (WCF) inclui três tipos de codificação para mensagens SOAP: O mecanismo de otimização de transmissão de mensagens e de texto, binário e mensagem (MTOM).  
  
 O `binaryMessageEncoding` elemento Especifica o formato binário .net para XML e tem opções para especificar a codificação de caracteres e a versão SOAP e WS-Addressing a ser usada. O codificador de mensagens binárias codifica as mensagens de Windows Communication Foundation (WCF) em binário na conexão. Embora essa codificação resulte em uma transmissão muito rápida de mensagens, a interoperabilidade baseada em padrões WS-* é perdida.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<binaryMessageEncoding maxReadPoolSize="211"
                       maxWritePoolSize="2132"
                       maxSessionSize="3141" />
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Configuration.BinaryMessageEncodingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>
- [Codificação de mensagens](message-encoding.md)
- [Escolhendo um codificador de mensagem](../../../wcf/feature-details/choosing-a-message-encoder.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
