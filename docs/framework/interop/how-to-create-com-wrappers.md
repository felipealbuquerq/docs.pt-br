---
title: 'Como: Criar wrappers COM'
ms.date: 03/30/2017
helpviewer_keywords:
- COM,wrappers creating
- COM,wrappers Visual Studio
ms.assetid: bdf89bea-1623-45ee-a57b-cf7c90395efa
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 7a6af73a5251cdc52589967178218f8675cac869
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69946471"
---
# <a name="how-to-create-com-wrappers"></a>Como: Criar wrappers COM

Você pode criar wrappers COM (Component Object Model) usando recursos do Visual Studio 2005 ou as ferramentas Tlbimp.exe e Regasm.exe do .NET Framework. Ambos os métodos geram dois tipos de wrappers COM:

- Um [RCW (Runtime Callable Wrapper)](../../standard/native-interop/runtime-callable-wrapper.md) de uma biblioteca de tipos para executar um objeto COM em um código gerenciado.

- Um [COM Callable Wrapper](../../standard/native-interop/com-callable-wrapper.md) com as configurações do Registro necessárias para executar um objeto gerenciado em um aplicativo nativo.

No Visual Studio 2005, você pode adicionar o wrapper COM como uma referência ao projeto.

## <a name="wrap-com-objects-in-a-managed-application"></a>Encapsular objetos COM em um aplicativo gerenciado

### <a name="to-create-a-runtime-callable-wrapper-using-visual-studio"></a>Para criar um RCW (Runtime Callable Wrapper) usando o Visual Studio

1. Abra o projeto do aplicativo gerenciado.

2. No menu **Projeto**, clique em **Mostrar Todos os Arquivos**.

3. No menu **Projeto**, clique em **Adicionar Referência**.

4. Na caixa de diálogo Adicionar Referência, clique na guia **COM**, selecione o componente que você deseja usar e clique em **OK**.

     No **Gerenciador de Soluções**, observe que o componente COM é adicionado à pasta Referências do projeto.

Agora você pode escrever o código para acessar o objeto COM. Comece declarando o objeto, por exemplo, com uma instrução `Imports` para o Visual Basic ou uma instrução `Using` para o C#.

> [!NOTE]
> Caso deseje programar componentes do Microsoft Office, primeiro instale os [PIAs (Assemblies de Interoperabilidade Primários) do Microsoft Office](https://go.microsoft.com/fwlink/?LinkId=50479) no Centro de Download da Microsoft. Na etapa 4, selecione a última versão da biblioteca de objetos disponível para o produto do Office desejado, como a **Biblioteca de Objetos do Microsoft Word 11.0**.  
  
### <a name="to-create-a-runtime-callable-wrapper-using-net-framework-tools"></a>Para criar um RCW (Runtime Callable Wrapper) usando ferramentas do .NET Framework  
  
- Execute a ferramenta [Tlbimp.exe (Importador da Biblioteca de Tipos)](../../../docs/framework/tools/tlbimp-exe-type-library-importer.md).  
  
 Essa ferramenta cria um assembly que contém metadados em tempo de execução para os tipos definidos na biblioteca de tipos original.  
  
## <a name="wrap-managed-objects-in-a-native-application"></a>Encapsular objetos gerenciados em um aplicativo nativo  
  
### <a name="to-create-a-com-callable-wrapper-using-visual-studio"></a>Para criar um COM Callable Wrapper usando o Visual Studio  
  
1. Crie um projeto de Biblioteca de Classes para a classe gerenciada que você deseja executar no código nativo. A classe deve ter um construtor sem parâmetros.  
  
     Verifique se você tem um número de versão de quatro partes completo para o assembly no arquivo AssemblyInfo. Esse número é necessário para manter o controle de versão no Registro do Windows. Para obter mais informações sobre os números de versão, consulte [Controle de versão do assembly](../../../docs/framework/app-domains/assembly-versioning.md).  
  
2. No menu **Projeto**, clique em **Propriedades**.  
  
3. Clique na guia **Compilar**.  
  
4. Marque a caixa de seleção **Registrar para interoperabilidade COM**.  
  
 Quando você cria o projeto, o assembly é registrado automaticamente para a interoperabilidade COM. Se estiver criando um aplicativo nativo no Visual Studio 2005, você poderá usar o assembly clicando em **Adicionar Referência** no menu **Projeto**.  
  
### <a name="to-create-a-com-callable-wrapper-using-net-framework-tools"></a>Para criar um COM Callable Wrapper usando ferramentas do .NET Framework  
  
Execute a ferramenta [Regasm.exe (Ferramenta de Registro de Assembly)](../../../docs/framework/tools/regasm-exe-assembly-registration-tool.md).  
  
Essa ferramenta lê os metadados do assembly e adiciona as entradas necessárias ao Registro. Como resultado, os clientes COM podem criar classes do .NET Framework de forma transparente. Use o assembly como se ele fosse uma classe COM nativa.  
  
Execute o Regasm.exe em um assembly localizado em qualquer diretório e, em seguida, execute o [Gacutil.exe (Ferramenta do Cache de Assembly Global)](../../../docs/framework/tools/gacutil-exe-gac-tool.md) para movê-lo para o cache de assembly global. A movimentação do assembly não invalida as entradas do Registro de local, porque o cache de assembly global sempre será examinado se o assembly não for encontrado em outro lugar.  
  
## <a name="see-also"></a>Consulte também

- [RCW (Runtime Callable Wrapper)](../../standard/native-interop/runtime-callable-wrapper.md)
- [COM Callable Wrapper](../../standard/native-interop/com-callable-wrapper.md)
