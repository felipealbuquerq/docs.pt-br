---
title: 'Como: criar um fluxo de trabalho de fluxograma'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 185d7aea-68a6-4bd8-adde-45050f33170a
ms.openlocfilehash: 3afac76fc977f992e780d2ebe302c1ed94568835
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70044408"
---
# <a name="how-to-create-a-flowchart-workflow"></a>Como: criar um fluxo de trabalho de fluxograma
Os fluxos de trabalho podem ser construídos a partir de atividades internas assim como as atividades personalizadas. Este tópico percorre a criação de um fluxo de trabalho que usa atividades internas, como a <xref:System.Activities.Statements.Flowchart> atividade, e as atividades personalizadas da seção [como: Criar um tópico](how-to-create-an-activity.md) de atividade. O fluxo de trabalho modela um jogo de palpite de número.  
  
> [!NOTE]
> Cada tópico do tutorial de Introdução depende dos tópicos anteriores. Para concluir este tópico, você deve primeiro concluir [como: Crie uma atividade](how-to-create-an-activity.md).  
  
> [!NOTE]
> Para baixar uma versão completa do tutorial, consulte [Windows Workflow Foundation (WF45) – introdução tutorial](https://go.microsoft.com/fwlink/?LinkID=248976).  
  
### <a name="to-create-the-workflow"></a>Para criar o fluxo de trabalho  
  
1. Clique com o botão direito do mouse em **NumberGuessWorkflowActivities** em **Gerenciador de soluções** e selecione **Adicionar**, **novo item**.  
  
2. No nó **itens comuns** **instalados**, selecione fluxo de **trabalho**. Selecione **atividade** na lista **fluxo de trabalho** .  
  
3. Digite `FlowchartNumberGuessWorkflow` na caixa **nome** e clique em **Adicionar**.  
  
4. Arraste uma atividade de **fluxograma** da seção **fluxograma** da **caixa de ferramentas** e solte-a no rótulo **soltar atividade aqui** na superfície de design do fluxo de trabalho.  
  
### <a name="to-create-the-workflow-variables-and-arguments"></a>Para criar as variáveis e os argumentos do fluxo de trabalho  
  
1. Clique duas vezes em **FlowchartNumberGuessWorkflow. XAML** em **Gerenciador de soluções** para exibir o fluxo de trabalho no designer, se ele ainda não estiver sendo exibido.  
  
2. Clique em **argumentos** no lado inferior esquerdo do designer de fluxo de trabalho para exibir o painel **argumentos** .  
  
3. Clique em **criar argumento**.  
  
4. Digite `MaxNumber` na caixa **nome** , selecione **em na** lista suspensa **direção** , selecione **Int32** na lista suspensa **tipo de argumento** e pressione ENTER para salvar o argumento...  
  
5. Clique em **criar argumento**.  
  
6. Digite `Turns` na caixa de **nome** que `MaxNumber` está abaixo do argumento recém-adicionado, selecione **fora** na lista suspensa **direção** , selecione **Int32** na lista suspensa tipo de **argumento** e, em seguida, pressione Enter.  
  
7. Clique em **argumentos** no lado inferior esquerdo do designer de atividade para fechar o painel **argumentos** .  
  
8. Clique em **variáveis** no lado inferior esquerdo do designer de fluxo de trabalho para exibir o painel **variáveis** .  
  
9. Clique em **criar variável**.  
  
    > [!TIP]
    > Se nenhuma caixa **criar variável** for exibida, clique na <xref:System.Activities.Statements.Flowchart> atividade na superfície do designer de fluxo de trabalho para selecioná-la.  
  
10. Digite `Guess` na caixa **nome** , selecione **Int32** na lista suspensa **tipo de variável** e pressione ENTER para salvar a variável.  
  
11. Clique em **criar variável**.  
  
12. Digite `Target` na caixa **nome** , selecione **Int32** na lista suspensa **tipo de variável** e pressione ENTER para salvar a variável.  
  
13. Clique em **variáveis** no lado inferior esquerdo do designer de atividade para fechar o painel **variáveis** .  
  
### <a name="to-add-the-workflow-activities"></a>Para adicionar as atividades de fluxo de trabalho  
  
1. Arraste uma atividade **atribuir** da seção **primitivos** da caixa de **ferramentas** e focalize-a sobre o nó **inicial** , que está na parte superior do fluxograma. Quando a atividade **assign** estiver sobre o nó **inicial** , três triângulos aparecerão em torno do nó **inicial** . Descarte a atividade **atribuir** no triângulo que está diretamente abaixo do nó de **início** . Isso vinculará os dois itens em conjunto e designará a atividade **atribuir** como a primeira atividade no fluxograma.  
  
    > [!NOTE]
    > As atividades também podem ser indicadas como a atividade de início no fluxo de trabalho manualmente vinculando-as ao nó de origem. Para fazer isso, passe o mouse sobre o nó **inicial** , clique em um dos retângulos que aparecem quando o mouse está sobre o nó **inicial** e arraste a linha de conexão para baixo até a atividade desejada e solte-a em um dos retângulos que aparecem. Você também pode designar uma atividade como a atividade inicial clicando com o botão direito do mouse nela e escolhendo **definir como nó inicial**.  
  
2. Digite `Target` na caixa **para** e a expressão a seguir na caixa **Inserir uma C# expressão** ou **Inserir uma expressão VB** .  
  
    ```vb  
    New System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    ```csharp  
    new System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    > [!TIP]
    > Se a janela **caixa de ferramentas** não for exibida, selecione caixa de **ferramentas** no menu **Exibir** .  
  
3. Arraste uma atividade de **prompt** da seção **NumberGuessWorkflowActivities** da **caixa de ferramentas**, solte-a abaixo da atividade **atribuir** da etapa anterior e conecte a atividade de **prompt** à atividade **atribuir** . Há três modos de conectar as duas atividades. A primeira maneira é conectá-los à medida que você remove a atividade **prompt** no fluxo de trabalho. Conforme você está arrastando a atividade de **prompt** para o fluxo de trabalho, passe o mouse sobre a atividade **atribuir** e solte-a em um dos quatro triângulos que aparecem quando a atividade de **prompt** está sobre a atividade **atribuir** . A segunda maneira é descartar a atividade de **prompt** no fluxo de trabalho no local desejado. Em seguida, passe o mouse sobre a atividade **atribuir** e arraste um dos retângulos que aparece até a atividade de **prompt** . Arraste o mouse para que a linha de conexão da atividade **atribuir** se conecte a um dos retângulos da atividade de **prompt** e, em seguida, solte o botão do mouse. A terceira maneira é muito semelhante à primeira maneira, exceto que, em vez de arrastar a atividade **prompt** da **caixa de ferramentas**, você a arrasta de seu local na superfície de design do fluxo de trabalho, passa o mouse sobre a atividade **atribuir** e a solta em uma das triângulos que aparece.  
  
4. Na **janela Propriedades** da atividade **prompt** , digite `"EnterGuess"` incluindo as aspas na caixa valor da propriedade **BookmarkName** . Digite `Guess` na caixa valor da propriedade de **resultado** e digite a expressão a seguir na caixa de propriedade **texto** .  
  
    ```vb  
    "Please enter a number between 1 and " & MaxNumber  
    ```  
  
    ```csharp  
    "Please enter a number between 1 and " + MaxNumber  
    ```  
  
    > [!TIP]
    > Se a **janela Propriedades** não for exibida, selecione **janela Propriedades** no menu **Exibir** .  
  
5. Arraste uma atividade **atribuir** da seção **primitivos** da caixa de **ferramentas** e conecte-a usando um dos métodos descritos na etapa anterior para que ela esteja abaixo da atividade de **prompt** .  
  
6. Digite `Turns` na caixa **para** e `Turns + 1` na caixa **Inserir uma C# expressão** ou **Inserir uma expressão VB** .  
  
7. Arraste um **FlowDecision** da seção **fluxograma** da caixa de **ferramentas** e conecte-o abaixo da atividade **atribuir** . Na **janela Propriedades**, digite a expressão a seguir na caixa valor da propriedade de **condição** .  
  
    ```vb  
    Guess = Target  
    ```  
  
    ```csharp  
    Guess == Target  
    ```  
  
8. Arraste outra atividade **FlowDecision** da **caixa de ferramentas** e solte-a abaixo da primeira. Conecte as duas atividades arrastando do retângulo rotulado como **falso** na atividade **FlowDecision** superior para o retângulo na parte superior da segunda atividade **FlowDecision** .  
  
    > [!TIP]
    > Se você não vir os rótulos **verdadeiro** e **falso** no **FlowDecision**, passe o mouse sobre o **FlowDecision**.  
  
9. Clique na segunda atividade **FlowDecision** para selecioná-la. Na **janela Propriedades**, digite a expressão a seguir na caixa valor da propriedade de **condição** .  
  
    ```
    Guess < Target  
    ```  
  
10. Arraste duas atividades **WriteLine** da seção **primitivas** da caixa de **ferramentas** e solte-as para que elas fiquem lado a lado abaixo das duas atividades **FlowDecision** . Conecte a ação **verdadeira** da atividade **FlowDecision** inferior à atividade **WriteLine** mais à esquerda e a ação **false** para a atividade **WriteLine** mais à direita.  
  
11. Clique na atividade **WriteLine** mais à esquerda para selecioná-la e digite a expressão a seguir na caixa valor da propriedade de **texto** na **janela Propriedades**.  
  
    ```
    "Your guess is too low."  
    ```  
  
12. Conecte o **WriteLine** ao lado esquerdo da atividade de **prompt** que está acima dele.  
  
13. Clique na atividade **WriteLine** mais à direita para selecioná-la e digite a expressão a seguir na caixa valor da propriedade de **texto** na **janela Propriedades**.  
  
    ```
    "Your guess is too high."  
    ```  
  
14. Conecte a atividade **WriteLine** ao lado direito da atividade de **prompt** acima dela.  
  
     O exemplo a seguir ilustra o fluxo de trabalho concluído.  
  
     ![Diagrama que mostra um fluxograma de Windows Workflow Foundation concluído.](./media/how-to-create-a-flowchart-workflow/completed-windows-workflow-flowchart.png)  
  
### <a name="to-build-the-workflow"></a>Para compilar o fluxo de trabalho  
  
1. Pressione CTRL+SHIFT+B para criar a solução.  
  
     Para obter instruções sobre como executar o fluxo de trabalho, consulte o próximo tópico [, como: Executar um fluxo](how-to-run-a-workflow.md)de trabalho. Se você já tiver concluído o [como: Execute uma etapa](how-to-run-a-workflow.md) de fluxo de trabalho com um estilo diferente de fluxo de trabalho e deseje executá-lo usando o fluxo de trabalho de fluxograma nesta etapa, pule para a seção [para criar e executar o aplicativo](how-to-run-a-workflow.md#BKMK_ToRunTheApplication) de [como: Executar um fluxo](how-to-run-a-workflow.md)de trabalho.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Activities.Statements.Flowchart>
- <xref:System.Activities.Statements.FlowDecision>
- [Programação do Windows Workflow Foundation](programming.md)
- [Criando fluxos de trabalho](designing-workflows.md)
- [Tutorial de Introdução](getting-started-tutorial.md)
- [Como: Criar uma atividade](how-to-create-an-activity.md)
- [Como: Executar um fluxo de trabalho](how-to-run-a-workflow.md)
