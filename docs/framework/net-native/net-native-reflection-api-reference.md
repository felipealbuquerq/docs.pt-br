---
title: Referência da API de reflexão nativa do .NET
ms.date: 03/30/2017
ms.assetid: 0429c049-22a3-4ba1-9cc8-f6ee91e31d9c
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 69a4addbd00c119af4336faae2cd0f8fc31f8852
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69941614"
---
# <a name="net-native-reflection-api-reference"></a>Referência da API de reflexão nativa do .NET
.NET Native inclui três novos tipos de exceção: [System. Runtime. compiladorservices. MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md), [System. Reflection. MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md)e [System. Reflection. MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md). Observe o seguinte sobre todos os três tipos de exceção:  
  
 Esses tipos destinam-se somente a uso interno.  
 Esses três tipos de exceção são para o uso somente da cadeia de ferramentas .NET Native. As exceções são geradas quando a cadeia de ferramentas de .NET Native detecta dados ausentes que não permitem que a execução do programa continue.  
  
 Não trate essas exceções em seu código.  
 Essas exceções indicam que os metadados necessários para seu aplicativo estão ausentes (as exceções [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md) e [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md)) ou que o código de implementação necessário para seu aplicativo está ausente (a exceção [MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md)). Corrija essas condições de exceção modificando um arquivo (.rd.xml) de diretivas de tempo de execução para tornar os metadados ou o código de implementação necessário disponíveis em tempo de execução. Para obter mais informações, consulte [Referência do arquivo de configuração de diretivas do tempo de execução (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md). Há duas soluções de problemas disponíveis que fornecem as entradas corretas para o arquivo de diretivas de tempo de execução que eliminará as exceções [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) e [MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md):  
  
- A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) para tipos.  
  
- A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) para métodos.  
  
> [!NOTE]
> Esta referência documenta três tipos de exceção que são exclusivos para .NET Native. Para obter a documentação de referência para a API de reflexão de <xref:System.Reflection>núcleo <xref:System.Reflection.Context> .NET Framework <xref:System.Reflection.Emit> , consulte os namespaces e. Para ver a documentação de referência da API de interoperabilidade principal do .NET Framework, consulte <xref:System.Runtime.InteropServices>.  
  
## <a name="systemreflection-namespace"></a>Namespace System.Reflection  
 O namespace <xref:System.Reflection> contém os tipos principais usados para reflexão do .NET Framework. Por .NET Native, ele também inclui dois novos tipos de exceção:  
  
|Classe|Descrição|  
|-----------|-----------------|  
|[MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md)|A exceção que é acionada quando reflexão é usada para recuperar metadados não presentes.|  
|[MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md)|A exceção que é acionada quando metadados de um tipo ou um membro de tipo estão disponíveis, mas sua implementação foi removida.|  
  
 Para ver documentação sobre outros tipos neste namespace, consulte as páginas de referência <xref:System.Reflection> no conjunto de documentação do .NET Framework.  
  
## <a name="systemruntimecompilerservices-namespace"></a>Namespace System.Runtime.CompilerServices  
 O namespace <xref:System.Runtime.CompilerServices> inclui tipos desenvolvidos para o usuário por compiladores de linguagem. Por .NET Native, ele também inclui um novo tipo de exceção:  
  
|Classe|Descrição|  
|-----------|-----------------|  
|[MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md)|A exceção que é acionada quando um método de marshaling manual é chamado, mas os metadados de um tipo não são encontrados por análise estática ou em um arquivo de diretivas de tempo de execução.|  
  
 Para ver documentação sobre outros tipos neste namespace, consulte as páginas de referência <xref:System.Runtime.CompilerServices> no conjunto de documentação do .NET Framework.  
  
## <a name="see-also"></a>Consulte também

- [Classe MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md)
- [Classe MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md)
- [Classe MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md)
- [Introdução](../../../docs/framework/net-native/getting-started-with-net-native.md)
