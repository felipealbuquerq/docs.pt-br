---
title: Guia de Introdução ao .NET Nativo
ms.date: 03/30/2017
ms.assetid: fc9e04e8-2d05-4870-8cd6-5bd276814afc
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: ce43ef1a24a0b9bcf0248647245cc35838700295
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69910495"
---
# <a name="getting-started-with-net-native"></a>Guia de Introdução ao .NET Nativo

Se você estiver escrevendo um novo aplicativo do Windows para o Windows 10 ou migrando um aplicativo existente da Windows Store, siga o mesmo conjunto de procedimentos. Para criar um aplicativo .NET Native, siga estas etapas:

1. [Desenvolver um aplicativo UWP (da Plataforma Universal do Windows) que direciona o Windows 10](#Step1) e testar os builds de depuração do aplicativo para garantir que ele funcione corretamente.

2. [Lidar com o uso de reflexão e serialização adicionais](#Step2).

3. [Implantar e testar os builds de versão do aplicativo](#Step3).

4. [Resolver manualmente os metadados ausentes](#Step4) e repetir a [etapa 3](#Step3) até que todos os problemas sejam resolvidos.

> [!NOTE]
> Se você estiver migrando um aplicativo da Windows Store existente para .NET Native, certifique-se de examinar a [migração de seu aplicativo da Windows Store para .net Native](../../../docs/framework/net-native/migrating-your-windows-store-app-to-net-native.md).

<a name="Step1"></a>

## <a name="step-1-develop-and-test-debug-builds-of-your-uwp-app"></a>Etapa 1: Desenvolver e testar compilações de depuração de seu aplicativo UWP

Se você estiver desenvolvendo um novo aplicativo ou migrando um existente, siga o mesmo processo de qualquer aplicativo do Windows.

1. Crie um novo projeto UWP no Visual Studio usando o modelo de aplicativo Universal do Windows para Visual C# ou Visual Basic. Por padrão, todos os aplicativos UWP direcionam o CoreCLR e seus builds de versão são compilados usando a cadeia de ferramentas do .NET Native.

2. Observe que há alguns problemas de compatibilidade conhecidos entre a compilação de projetos de aplicativo UWP com a cadeia de ferramentas do .NET Native e sem ela. Consulte o [guia de migração](../../../docs/framework/net-native/migrating-your-windows-store-app-to-net-native.md) para obter mais informações.

Agora você pode gravar C# ou Visual Basic código em relação à área de superfície de .net Native que é executada no sistema local (ou no simulador).

> [!IMPORTANT]
> Ao desenvolver seu aplicativo, observe qualquer uso de serialização ou reflexão no seu código.

Por padrão, as compilações de depuração são compiladas por JIT para habilitar a implantação rápida de F5, enquanto as compilações de versão são compiladas usando a tecnologia de pré-compilação .NET Native. Isso significa que você deve criar e testar os builds de depuração do aplicativo para garantir que eles funcionem normalmente antes de compilá-los com a cadeia de ferramentas do .NET Native.

<a name="Step2"></a>

## <a name="step-2-handle-additional-reflection-and-serialization-usage"></a>Etapa 2: Manipular o uso adicional e a serialização

Um arquivo de diretivas de tempo de execução, Default.rd.xml, é adicionado automaticamente ao projeto quando ele é criado. Se você desenvolvê-lo no C#, ele será encontrado na pasta **Propriedades** do projeto. Se você desenvolvê-lo no Visual Basic, ele será encontrado na pasta **Meu Projeto** do projeto.

> [!NOTE]
> Para obter uma visão geral do processo de compilação do .NET Native que fornece informações de contexto sobre a necessidade de ter um arquivo das diretivas de tempo de execução, consulte [.NET Native e compilação](../../../docs/framework/net-native/net-native-and-compilation.md).

O arquivo de diretivas de tempo de execução é usado para definir os metadados de que o aplicativo precisa em tempo de execução. Em alguns casos, a versão padrão do arquivo pode ser adequada. No entanto, um código que depende da serialização ou da reflexão pode exigir entradas adicionais no arquivo de diretivas de tempo de execução.

**Serialização**

Há duas categorias de serializadores e ambas podem necessitar de entradas adicionais no arquivo de diretivas de tempo de execução:

- Serializadores não baseado em reflexão. Os serializadores encontrados na biblioteca de classes do .NET Framework, como as classes <xref:System.Runtime.Serialization.DataContractSerializer>, <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> e <xref:System.Xml.Serialization.XmlSerializer>, não dependem de reflexão. No entanto, eles necessitam que o código seja gerado com base no objeto a ser serializado ou desserializado.  Para obter mais informações, consulte a seção “Serializadores da Microsoft” em [Serialização e metadados](../../../docs/framework/net-native/serialization-and-metadata.md).

- Serializadores de terceiros. As bibliotecas de serialização de terceiros, a mais comum do que é o serializador JSON Newtonsoft, geralmente são baseadas em reflexão e exigem entradas \*no arquivo. Rd. xml para dar suporte à serialização e desserialização de objetos. Para obter mais informações, consulte a seção “Serializadores de Terceiros” em [Serialização e metadados](../../../docs/framework/net-native/serialization-and-metadata.md).

**Métodos que dependem de reflexão**

Em alguns casos, o uso de reflexão no código não é óbvio. Algumas APIs comuns ou padrões de programação não são consideradas como parte da API de reflexão, mas dependem dela para serem executados com êxito. Isso inclui os seguintes métodos de instanciação do tipo e de construção de método:

- O método <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType>

- Os métodos <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> e <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType>

- O método <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType>.

Para obter mais informações, consulte [APIs que dependem de reflexão](../../../docs/framework/net-native/apis-that-rely-on-reflection.md).

> [!NOTE]
> Nomes de tipo usados em arquivos de diretivas de tempo de execução devem ser totalmente qualificados. Por exemplo, o arquivo deve especificar "System.String" em vez de "String".

<a name="Step3"></a>

## <a name="step-3-deploy-and-test-the-release-builds-of-your-app"></a>Etapa 3: Implantar e testar as compilações de versão do seu aplicativo

Depois de atualizar o arquivo de diretivas de tempo de execução, você poderá recompilar e implantar builds de versão do aplicativo. Os binários do .NET Native são colocados no subdiretório ILC.out do diretório especificado na caixa de texto **Caminho de saída do build** da caixa de diálogo **Propriedades** do projeto, guia **Compilar**. Binários que não estão nessa pasta não foram compilados com .NET Nativo. Teste o aplicativo por completo e todos os cenários, incluindo cenários de falha, em cada uma de suas plataformas de destino.

Se seu aplicativo não funcionar corretamente (particularmente em casos em que ele lança exceções [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) ou [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md) em tempo de execução), siga as instruções na próxima seção [, etapa 4: Resolva manualmente os metadados](#Step4)ausentes. Ativar exceções de primeira chance pode ajudá-lo a encontrar esses bugs.

Quando você tiver testado e depurado as compilações de depuração do seu aplicativo e tiver certeza de que eliminou as exceções de [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) e [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md) , deverá testar seu aplicativo como um aplicativo de .net Native otimizado. Para fazer isso, altere as configurações do projeto ativo de **Depuração** para **Versão**.

<a name="Step4"></a>

## <a name="step-4-manually-resolve-missing-metadata"></a>Etapa 4: Resolver manualmente os metadados ausentes

A falha mais comum que você encontrará com .NET Native que você não encontrar na área de trabalho é uma exceção de tempo de execução [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md), [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md)ou [MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md) . Em alguns casos, a ausência de metadados pode manifestar-se em um comportamento imprevisível ou mesmo em falhas de aplicativo. Esta seção discute como depurar e resolver essas exceções adicionando diretivas ao arquivo de diretivas de tempo de execução. Para obter informações sobre o formato das diretivas de tempo de execução, consulte [Referência do arquivo de configuração das diretivas de tempo de execução (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md). Depois de adicionar as diretivas de tempo de execução, você deverá [implantar e testar o aplicativo](#Step3) novamente e resolver todas as novas exceções [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md), [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md) e [MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md) até não encontrar mais exceções.

> [!TIP]
> Especifique as diretivas de tempo de execução em um nível superior para permitir que seu aplicativo seja resistente a alterações de código.  É recomendável adicionar diretivas de tempo de execução nos níveis de namespace e tipo em vez de nível do membro. Observe que pode haver uma relação entre resiliência e binários maiores com mais tempo de compilação.

Ao lidar com uma exceção de metadados ausentes, considere esses problemas:

- O que o aplicativo estava tentando fazer antes da exceção?

  - Por exemplo, ele estava associando dados, serializando ou desserializando dados ou usando diretamente a API de reflexão?

- Trata-se de um caso isolado ou você acredita que encontrará o mesmo problema para outros tipos?

  - Por exemplo, uma exceção [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) é gerada ao serializar um tipo no modelo de objeto do aplicativo.  Se você souber que outros tipos serão serializados, poderá adicionar diretivas de tempo de execução para esses tipos (ou para seus namespaces recipientes, dependendo de como o código é organizado) ao mesmo tempo.

- É possível reescrever o código para que ele não use reflexão?

  - Por exemplo, o código usa a palavra-chave `dynamic` quando você sabe qual tipo esperar?

  - O código chama um método que depende de reflexão quando alguma alternativa melhor está disponível?

> [!NOTE]
> Para obter informações adicionais sobre como lidar com problemas que se originam de diferenças na reflexão e a disponibilidade de metadados em aplicativos de desktop e .NET Native, consulte [APIs que dependem da reflexão](../../../docs/framework/net-native/apis-that-rely-on-reflection.md).

Para ver alguns exemplos específicos de como lidar com exceções e outros problemas que ocorrem ao testar seu aplicativo, consulte:

- [Exemplo: Tratamento de exceções ao associar dados](../../../docs/framework/net-native/example-handling-exceptions-when-binding-data.md)

- [Exemplo: Solucionando problemas de programação dinâmica](../../../docs/framework/net-native/example-troubleshooting-dynamic-programming.md)

- [Exceções em tempo de execução em aplicativos do .NET Native](../../../docs/framework/net-native/runtime-exceptions-in-net-native-apps.md)

## <a name="see-also"></a>Consulte também

- [Referência do arquivo de configuração das diretivas de tempo de execução (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)
- [Instalação e configuração do .NET Native](https://docs.microsoft.com/previous-versions/dn600164(v=vs.110))
- [.NET Native e compilação](../../../docs/framework/net-native/net-native-and-compilation.md)
- [Reflexão e .NET Native](../../../docs/framework/net-native/reflection-and-net-native.md)
- [APIs que dependem de reflexão](../../../docs/framework/net-native/apis-that-rely-on-reflection.md)
- [Serialização e metadados](../../../docs/framework/net-native/serialization-and-metadata.md)
- [Migrando seu aplicativo da Windows Store para .NET Native](../../../docs/framework/net-native/migrating-your-windows-store-app-to-net-native.md)
