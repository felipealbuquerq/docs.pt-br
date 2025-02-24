---
title: 'Como: validar configurações de aplicativo'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- validating application settings
- application settings [Windows Forms], Windows Forms
- application settings [Windows Forms], validating
ms.assetid: 9f145ada-4267-436a-aa4c-c4dcffd0afb7
ms.openlocfilehash: 220b86c0de57e60036527bb49f2d8de46390a9ed
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69929785"
---
# <a name="how-to-validate-application-settings"></a>Como: validar configurações de aplicativo

Este tópico demonstra como validar configurações do aplicativo antes que elas sejam persistentes.

Considerando que as configurações do aplicativo são fortemente tipadas, você tem alguma confiança de que os usuários não podem atribuir dados de um tipo incorreto a uma determinada configuração. No entanto, um usuário ainda pode tentar atribuir um valor a uma configuração que esteja fora do limite aceitável, por exemplo, fornecendo uma data de nascimento no futuro. <xref:System.Configuration.ApplicationSettingsBase>, a classe pai de todas as classes de configurações de aplicativo, expõe quatro eventos para habilitar essa verificação de limites. Manipular esses eventos coloca todo o código de validação em um único local, em vez de distribuí-lo em todo o projeto.

O evento que você usa depende de quando você precisa validar suas configurações, conforme descrito na tabela a seguir.

|evento|Ocorrência e uso|
|-----------|------------------------|
|<xref:System.Configuration.ApplicationSettingsBase.SettingsLoaded>|Ocorre após o carregamento inicial de um grupo de propriedades de configurações.<br /><br /> Use esse evento para validar valores iniciais para todo o grupo de propriedades antes que eles sejam usados no aplicativo.|
|<xref:System.Configuration.ApplicationSettingsBase.SettingChanging>|Ocorre antes da alteração do valor de uma única propriedade.<br /><br /> Use esse evento para validar uma única propriedade antes que ela seja alterada. Pode fornecer imediatamente comentários aos usuários sobre suas ações e escolhas.|
|<xref:System.Configuration.ApplicationSettingsBase.PropertyChanged>|Ocorre depois da alteração de uma única propriedade de configurações.<br /><br /> Use esse evento para validar uma única propriedade depois que ela for alterada. Este evento raramente é usado para validação, a menos que um processo de validação longo e assíncrono seja necessário.|
|<xref:System.Configuration.ApplicationSettingsBase.SettingsSaving>|Ocorre antes que o grupo de propriedades de configurações seja armazenado.<br /><br /> Use esse evento para validar valores para todo o grupo de propriedades antes que elas sejam persistentes no disco.|

Normalmente, você não usará todos esses eventos dentro do mesmo aplicativo para fins de validação. Por exemplo, geralmente é possível atender a todos os requisitos de validação manipulando apenas <xref:System.Configuration.ApplicationSettingsBase.SettingChanging> o evento.

Um manipulador de eventos geralmente executa uma das seguintes ações quando detecta um valor inválido:

- Fornece automaticamente um valor sabidamente correto, como o valor padrão.

- Consulta novamente o usuário do código do servidor para obter informações.

- Para eventos gerados antes de suas ações associadas, <xref:System.Configuration.ApplicationSettingsBase.SettingChanging> como e <xref:System.Configuration.ApplicationSettingsBase.SettingsSaving>, o usa o <xref:System.ComponentModel.CancelEventArgs> argumento para cancelar a operação.

Para obter mais informações sobre a manipulação de eventos, consulte [Visão geral de manipuladores de evento](../event-handlers-overview-windows-forms.md).

Os procedimentos a seguir mostram como testar uma data de nascimento válida usando o <xref:System.Configuration.ApplicationSettingsBase.SettingChanging> ou o <xref:System.Configuration.ApplicationSettingsBase.SettingsSaving> evento. Os procedimentos foram escritos com a pressuposição de que você já criou suas configurações do aplicativo. Neste exemplo, vamos executar a verificação de limites em uma configuração chamada `DateOfBirth`. Para obter mais informações sobre como criar configurações [, consulte Como: Criar configurações](how-to-create-application-settings.md)do aplicativo.

### <a name="to-obtain-the-application-settings-object"></a>Para obter o objeto de configurações do aplicativo

- Obtenha uma referência para o objeto de configurações de aplicativo (a instância do wrapper) executando um dos seguintes itens com marcadores:

  - Se você tiver criado suas configurações usando a caixa de diálogo de Configurações de aplicativo do Visual Studio no **Editor de propriedade**, poderá recuperar o objeto de configurações padrão gerado para sua linguagem por meio da seguinte expressão.

    ```csharp
    Configuration.Settings.Default
    ```

    ```vb
    MySettings.Default
    ```

    - ou -

  - Se você for um desenvolvedor de Visual Basic e tiver criado as configurações do aplicativo usando o Designer de Projeto, poderá recuperar as configurações usando o [Objeto My.Settings](../../../visual-basic/language-reference/objects/my-settings-object.md).

    - ou -

  - Se você criou suas configurações derivando <xref:System.Configuration.ApplicationSettingsBase> diretamente, você precisa criar uma instância da classe manualmente.

    ```csharp
    MyCustomSettings settings = new MyCustomSettings();
    ```

    ```vb
    Dim Settings as New MyCustomSettings()
    ```

Os procedimentos a seguir foram escritos com a suposição de que o objeto de configurações do aplicativo foi obtido concluindo o último item com marcadores neste procedimento.

### <a name="to-validate-application-settings-when-a-setting-is-changing"></a>Para validar as Configurações do Aplicativo quando uma configuração é alterada

1. Se você for um C# desenvolvedor, no evento do formulário ou do `Load` controle, adicione um manipulador de eventos para <xref:System.Configuration.ApplicationSettingsBase.SettingChanging> o evento.

    - ou -

    Se você for um desenvolvedor de Visual Basic, deverá declarar a variável `Settings` usando a palavra-chave `WithEvents`.

    ```csharp
    public void Form1_Load(Object sender, EventArgs e)
    {
        settings.SettingChanging += new SettingChangingEventHandler(MyCustomSettings_SettingChanging);
    }
    ```

    ```vb
    Public Sub Form1_Load(sender as Object, e as EventArgs)
        AddHandler settings.SettingChanging, AddressOf MyCustomSettings_SettingChanging
    End Sub
    ```

2. Defina o manipulador de eventos e escreva o código dentro dele para executar a verificação de limites na data de nascimento.

    ```csharp
    private void MyCustomSettings_SettingChanging(Object sender, SettingChangingEventArgs e)
    {
        if (e.SettingName.Equals("DateOfBirth")) {
            Date newDate = (Date)e.NewValue;
            If (newDate > Date.Now) {
                e.Cancel = true;
                // Inform the user.
            }
        }
    }
    ```

    ```vb
    Private Sub MyCustomSettings_SettingChanging(sender as Object, e as SettingChangingEventArgs) Handles Settings.SettingChanging
        If (e.SettingName.Equals("DateOfBirth")) Then
            Dim NewDate as Date = CType(e.NewValue, Date)
            If (NewDate > Date.Now) Then
                e.Cancel = True
                ' Inform the user.
            End If
        End If
    End Sub
    ```

### <a name="to-validate-application-settings-when-a-save-occurs"></a>Para validar as Configurações do Aplicativo quando uma operação de Salvar ocorrer

1. No evento do formulário ou do `Load` controle, adicione um manipulador de eventos para <xref:System.Configuration.ApplicationSettingsBase.SettingsSaving> o evento.

    ```csharp
    public void Form1_Load(Object sender, EventArgs e)
    {
        settings.SettingsSaving += new SettingsSavingEventHandler(MyCustomSettings_SettingsSaving);
    }
    ```

    ```vb
    Public Sub Form1_Load(Sender as Object, e as EventArgs)
        AddHandler settings.SettingsSaving, AddressOf MyCustomSettings_SettingsSaving
    End Sub
    ```

2. Defina o manipulador de eventos e escreva o código dentro dele para executar a verificação de limites na data de nascimento.

    ```csharp
    private void MyCustomSettings_SettingsSaving(Object sender, SettingsSavingEventArgs e)
    {
        if (this["DateOfBirth"] > Date.Now) {
            e.Cancel = true;
        }
    }
    ```

    ```vb
    Private Sub MyCustomSettings_SettingsSaving(Sender as Object, e as SettingsSavingEventArgs)
        If (Me["DateOfBirth"] > Date.Now) Then
            e.Cancel = True
        End If
    End Sub
    ```

## <a name="see-also"></a>Consulte também

- [Criando manipuladores de eventos no Windows Forms](../creating-event-handlers-in-windows-forms.md)
- [Como: Criar configurações de aplicativo](how-to-create-application-settings.md)
