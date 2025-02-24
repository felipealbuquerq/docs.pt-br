---
title: 'Como: responder a alterações no esquema de fontes em um aplicativo do Windows Forms'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, font scheme changes
ms.assetid: 4db27702-22e7-43bf-a07d-9a004549853c
ms.openlocfilehash: 9fd7f99b35730cf867bfad5da24bc3f223e9a0f8
ms.sourcegitcommit: 9b1ac36b6c80176fd4e20eb5bfcbd9d56c3264cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67425329"
---
# <a name="how-to-respond-to-font-scheme-changes-in-a-windows-forms-application"></a>Como: responder a alterações no esquema de fontes em um aplicativo do Windows Forms
Nos sistemas operacionais Windows, um usuário pode alterar as configurações de fonte de todo o sistema para tornar a fonte padrão maior ou menor. A alteração dessas configurações de fonte é essencial para usuários com deficiência visual, que requerem letras maiores para ler o texto em suas telas. É possível ajustar seu aplicativo do Windows Forms para reagir a essas alterações aumentando ou diminuindo o tamanho do formulário e todo o texto contido nele sempre que o esquema de fontes for alterado. Se desejar que seu formulário acomode as alterações em tamanhos de fonte dinamicamente, será possível adicionar código a ele.  
  
 Normalmente, a fonte padrão usada pelo Windows Forms é a fonte retornada pela <xref:Microsoft.Win32> chamada de namespace para `GetStockObject(DEFAULT_GUI_FONT)`. A fonte retornada por essa chamada muda apenas quando a resolução da tela é alterada. Conforme mostrado no procedimento a seguir, o código deve alterar a fonte padrão para <xref:System.Drawing.SystemFonts.IconTitleFont%2A> para responder a alterações no tamanho da fonte.  
  
### <a name="to-use-the-desktop-font-and-respond-to-font-scheme-changes"></a>Utilizar a fonte da área de trabalho e responder a alterações no esquema de fontes  
  
1. Crie seu formulário e adicione os controles desejados a ele. Para obter mais informações, confira [Como: Criar um aplicativo de formulários do Windows da linha de comando](how-to-create-a-windows-forms-application-from-the-command-line.md) e [controles a serem usados nos Windows Forms](./controls/controls-to-use-on-windows-forms.md).  
  
2. Adicione uma referência para o <xref:Microsoft.Win32> namespace ao seu código.  
  
     [!code-csharp[WinFormsAutoScaling#2](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#2)]
     [!code-vb[WinFormsAutoScaling#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#2)]  
  
3. Adicione o seguinte código ao construtor do formulário para ligar manipuladores de eventos necessários e para alterar a fonte padrão em uso do formulário.  
  
     [!code-csharp[WinFormsAutoScaling#3](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#3)]
     [!code-vb[WinFormsAutoScaling#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#3)]  
  
4. Implemente um manipulador para o <xref:Microsoft.Win32.SystemEvents.UserPreferenceChanged> evento que faz com que o formulário dimensionar automaticamente quando o <xref:Microsoft.Win32.UserPreferenceCategory.Window> as alterações de categoria.  
  
     [!code-csharp[WinFormsAutoScaling#4](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#4)]
     [!code-vb[WinFormsAutoScaling#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#4)]  
  
5. Finalmente, implemente um manipulador para o <xref:System.Windows.Forms.Form.FormClosing> evento que dispara o <xref:Microsoft.Win32.SystemEvents.UserPreferenceChanged> manipulador de eventos.  
  
     > [!IMPORTANT]
     > Uma falha na inclusão desse código fará com que ocorra vazamento de memória no aplicativo.  
  
     [!code-csharp[WinFormsAutoScaling#5](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#5)]
     [!code-vb[WinFormsAutoScaling#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#5)]  
  
6. Compile e execute o código.  
  
### <a name="to-manually-change-the-font-scheme-in-windows-xp"></a>Alterar manualmente o esquema de fontes no Windows XP  
  
1. Enquanto o Aplicativo do Windows Forms está em execução, clique com o botão direito do mouse na área de trabalho do Windows e escolha **Propriedades** no menu de atalho.  
  
2. Na caixa de diálogo **Propriedades de Exibição**, clique na guia **Aparência**.  
  
3. Na caixa de listagem suspensa **Tamanho da Fonte**, selecione um novo tamanho da fonte.  
  
     Você observará que o formulário agora reage a alterações de tempo de execução no esquema de fontes da área de trabalho. Quando o usuário altera entre **Normal**, **Fontes Grandes** e **Fontes Extra Grandes**, o formulário muda a fonte e ajusta a escala corretamente.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[WinFormsAutoScaling#1](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#1)]
 [!code-vb[WinFormsAutoScaling#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#1)]  
  
 O construtor neste exemplo de código contém uma chamada para `InitializeComponent`, que é definida quando você cria um novo projeto Windows Forms no Visual Studio. Remova essa linha de código se estiver compilando um aplicativo na linha de comando.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.ContainerControl.PerformAutoScale%2A>
- [Dimensionamento automático no Windows Forms](automatic-scaling-in-windows-forms.md)
