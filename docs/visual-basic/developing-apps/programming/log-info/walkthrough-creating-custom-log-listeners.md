---
title: Criar ouvintes de log personalizados (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- custom log listeners
- My.Application.Log object, custom log listeners
ms.assetid: 0e019115-4b25-4820-afb1-af8c6e391698
ms.openlocfilehash: 90135074a4d34ea73743faffb2531305fcb326fb
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69965268"
---
# <a name="walkthrough-creating-custom-log-listeners-visual-basic"></a>Passo a passo: Criar ouvintes de log personalizados (Visual Basic)
Estas instruções passo a passo demonstram como criar um ouvinte de log personalizado e configurá-lo para ouvir a saída do objeto `My.Application.Log`.  
  
## <a name="getting-started"></a>Guia de Introdução  
 Ouvintes de log devem herdar da classe <xref:System.Diagnostics.TraceListener>.  
  
#### <a name="to-create-the-listener"></a>Para criar o ouvinte  
  
- Em seu aplicativo, crie uma classe denominada `SimpleListener` que herda de <xref:System.Diagnostics.TraceListener>.  
  
     [!code-vb[VbVbalrMyApplicationLog#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#16)]  
  
     Os métodos <xref:System.Diagnostics.TraceListener.Write%2A> e <xref:System.Diagnostics.TraceListener.WriteLine%2A>, exigidos pela classe base, chamam `MsgBox` para exibir sua entrada.  
  
     O atributo <xref:System.Security.Permissions.HostProtectionAttribute> é aplicado aos métodos <xref:System.Diagnostics.TraceListener.Write%2A> e <xref:System.Diagnostics.TraceListener.WriteLine%2A> para que seus atributos coincidam com os métodos da classe base. O atributo <xref:System.Security.Permissions.HostProtectionAttribute> permite que o host que executa o código determine que o código expõe a sincronização de proteção de host.  
  
    > [!NOTE]
    > O atributo <xref:System.Security.Permissions.HostProtectionAttribute> é eficaz somente em aplicativos não gerenciados que hospedam o Common Language Runtime e implementam a proteção de host, como o SQL Server.  
  
 Para garantir que o `My.Application.Log` use o ouvinte de log, você deve nomear fortemente o assembly que contém o ouvinte de log.  
  
 O procedimento seguinte fornece algumas etapas simples para criar um assembly de ouvinte de log de nome forte. Para obter mais informações, consulte [Criando e usando assemblies de nomes fortes](../../../../framework/app-domains/create-and-use-strong-named-assemblies.md).  
  
#### <a name="to-strongly-name-the-log-listener-assembly"></a>Para nomear fortemente o assembly de ouvinte de log  
  
1. Selecione um projeto no **Gerenciador de Soluções**. No menu **Projeto**, escolha **Propriedades**.   
  
2. Clique na guia **Assinatura**.  
  
3. Marque a caixa **Assinar o assembly**.  
  
4. Selecione **Novo\<** da lista suspensa **Escolha um arquivo de chave de nome forte**.  
  
     A caixa de diálogo **Criar Chave de Nome Forte** é aberta.  
  
5. Forneça um nome para o arquivo de chaves na caixa **Nome de arquivo de chaves**.  
  
6. Digite uma senha nas caixas **Digite a senha** e **Confirmar senha** caixas.  
  
7. Clique em **OK**.  
  
8. Recompile o aplicativo.  
  
## <a name="adding-the-listener"></a>Adicionando o ouvinte  
 Agora que o assembly tem um nome forte, você precisa determinar o nome forte do ouvinte para que `My.Application.Log` use seu ouvinte de log.  
  
 O formato de um tipo de nome forte é o seguinte.  
  
 \<nome do tipo>, \<nome do assembly>, \<número de versão>, \<cultura>, \<nome forte>  
  
#### <a name="to-determine-the-strong-name-of-the-listener"></a>Para determinar o nome forte do ouvinte  
  
- O código a seguir mostra como determinar o tipo de nome forte de `SimpleListener`.  
  
     [!code-vb[VbVbalrMyApplicationLog#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#17)]  
  
     O nome forte do tipo depende de seu projeto.  
  
 Com o nome forte, você pode adicionar o ouvinte à coleção de ouvintes de log `My.Application.Log`.  
  
#### <a name="to-add-the-listener-to-myapplicationlog"></a>Para adicionar o ouvinte a My.Application.Log  
  
1. Clique com o botão direito do mouse em app.config no **Gerenciador de Soluções** e escolha **Abrir**.  
  
     -ou-  
  
     Se houver um arquivo app.config:  
  
    1. No menu **Projeto**, escolha **Adicionar Novo Item**.  
  
    2. Na caixa de diálogo **Adicionar novo item**, escolha **Arquivo de configuração de aplicativo**.  
  
    3. Clique em **Adicionar**.  
  
2. Localize a seção `<listeners>`, na seção `<source>` com o `name` atributo "DefaultSource", localizado na seção `<sources>`. A seção `<sources>` está localizada na seção `<system.diagnostics>`, na seção `<configuration>` superior.  
  
3. Adicione esse elemento à seção `<listeners>`:  
  
    ```xml  
    <add name="SimpleLog" />  
    ```  
  
4. Localize a seção `<sharedListeners>`, na seção `<system.diagnostics>`, na seção `<configuration>` superior.  
  
5. Adicione esse elemento a essa seção `<sharedListeners>`:  
  
    ```xml  
    <add name="SimpleLog" type="SimpleLogStrongName" />  
    ```  
  
     Altere o valor de `SimpleLogStrongName` para ser o nome forte do ouvinte.  
  
## <a name="see-also"></a>Consulte também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- [Trabalhando com logs de aplicativo](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)
- [Como: registrar exceções](../../../../visual-basic/developing-apps/programming/log-info/how-to-log-exceptions.md)
- [Como: gravar mensagens de log](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)
- [Passo a passo: alterando onde My.Application.Log grava informações](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)
