---
title: Solução de problemas gerais do .NET Nativo
ms.date: 03/30/2017
ms.assetid: ee8c5e17-35ea-48a1-8767-83298caac1e8
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9599ee25e77bf6f44e5c6733deed8dc23fc0d022
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67662333"
---
# <a name="net-native-general-troubleshooting"></a>Solução de problemas gerais do .NET Nativo

Este tópico descreve como solucionar problemas potenciais que podem ocorrer durante o desenvolvimento de aplicativos com o .NET Native.

- **Problema:** A janela de saída de compilação não é atualizada corretamente.

  **Resolução:** A janela de saída de compilação não será atualizada até que a compilação for concluída. Os tempos de compilação podem levar vários minutos, por isso pode ocorrer um atraso para exibir as atualizações.

- **Problema:** Tempo de build de varejo do seu aplicativo para ARM aumentou.

  **Resolução:** Quando você implanta um aplicativo em seu dispositivo ARM, a infraestrutura do .NET Native é invocada. Essa compilação executa um grande número de otimizações, garantindo que semânticas não estáticas como reflexão continuem a funcionar. Além disso, a parte do .NET Framework que o aplicativo usa está vinculada estaticamente para otimizar o desempenho e também precisa ser compilada em código nativo. É por isso que a compilação leva mais tempo.

  No entanto, o tempo de compilação ainda está dentro de um minuto da compilação padrão para a maioria dos aplicativos em uma máquina de desenvolvimento padrão.  Normalmente, apenas gerar imagens nativas para o .NET Framework em uma máquina de desenvolvimento padrão leva vários minutos.  Mesmo com todas as otimizações para melhorar o código gerado e incluindo o .NET Framework, os tempos de compilação do aplicativo são normalmente de um ou dois minutos.

  Continuamos a trabalhar para aprimorar o desempenho da compilação investigando a multi-threaded compilação e outras otimizações.

- **Problema:** Você não sabe se seu aplicativo foi compilado usando o .NET Native.

  **Resolução:** Se o compilador do .NET nativo é invocado, você perceberá que mais tempos de compilação e o Gerenciador de tarefas mostrará vários processos de componente nativo do .NET como ILC.exe e nutc_driver.exe.

  Depois de compilar seu projeto com o .NET Native com êxito, você verá a saída em obj\\*config*\ *arch*\\*projectname*. ilc\out.  O conteúdo do pacote nativo final pode ser encontrado em bin\\*arch*\\*config*\AppX. O conteúdo do pacote nativo final estará em \bin\\*arch*\\*config*\AppX se você tiver implantado o aplicativo.

- **Problema:** Seu aplicativo compilado com .NET Native está lançando exceções de tempo de execução (normalmente [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md) ou [MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md) exceções) que ele não lançava quando era compilado sem. NET nativo.

  **Resolução:** As exceções são geradas porque o .NET Native não forneceu os metadados ou o código de implementação que normalmente fica disponível por meio de reflexão. (Para obter mais informações, consulte [.NET Native e Compilação](../../../docs/framework/net-native/net-native-and-compilation.md).) Para eliminar a exceção, você precisa adicionar uma entrada ao seu [arquivo (rd.xml) de diretivas de tempo de execução](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md) para que a cadeia de ferramentas do .NET Native possa tornar o código de implementação ou os metadados disponíveis em tempo de execução. Há duas soluções de problemas disponíveis que criarão a entrada necessária para adicionar ao seu arquivo de diretivas de tempo de execução:

  - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/type.html) para tipos.

  - A [solução de problemas MissingMetadataException](https://dotnet.github.io/native/troubleshooter/method.html) para métodos.

  Para obter mais informações, consulte [Reflexão e .NET Native](../../../docs/framework/net-native/reflection-and-net-native.md).

## <a name="see-also"></a>Consulte também

- [Migrando seu aplicativo da Windows Store para .NET Native](../../../docs/framework/net-native/migrating-your-windows-store-app-to-net-native.md)
