---
title: 'Como: Carregar arquivos no controle RichTextBox do Windows Forms'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- text boxes [Windows Forms], displaying files
- examples [Windows Forms], text boxes
- .rtf files [Windows Forms], opening in RichTextBox control
- RTF files [Windows Forms], opening in RichTextBox control
- text files [Windows Forms], displaying in RichTextBox control
- .rtf files [Windows Forms], displaying in RichTextBox control
- RichTextBox control [Windows Forms], opening files
- RTF files [Windows Forms], displaying in RichTextBox control
ms.assetid: c03451be-f285-4428-a71a-c41e002cc919
ms.openlocfilehash: 0f52b4ff869d7a2220dd2d40e0ab90bbfb7d24ae
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70046177"
---
# <a name="how-to-load-files-into-the-windows-forms-richtextbox-control"></a>Como: Carregar arquivos no controle RichTextBox do Windows Forms

O controle <xref:System.Windows.Forms.RichTextBox> de Windows Forms pode exibir um arquivo de texto sem formatação de texto simples, Unicode ou RTF (Rich-Text-Format). Para fazer isso, chame o <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> método. Você também pode usar o <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> método para carregar dados de um fluxo. Para obter mais informações, consulte <xref:System.Windows.Forms.RichTextBox.LoadFile%28System.IO.Stream%2CSystem.Windows.Forms.RichTextBoxStreamType%29>.

### <a name="to-load-a-file-into-the-richtextbox-control"></a>Para carregar um Arquivo no controle RichTextBox

1. Determine o caminho do arquivo a ser aberto usando o <xref:System.Windows.Forms.OpenFileDialog> componente. Para obter uma visão geral, consulte [Visão geral do componente OpenFileDialog](openfiledialog-component-overview-windows-forms.md).

2. Chame o <xref:System.Windows.Forms.RichTextBox.LoadFile%2A> método <xref:System.Windows.Forms.RichTextBox> do controle, especificando o arquivo a ser carregado e, opcionalmente, um tipo de arquivo. No exemplo a seguir, o arquivo a ser carregado é obtido da <xref:System.Windows.Forms.OpenFileDialog> Propriedade do <xref:System.Windows.Forms.FileDialog.FileName%2A> componente. Se você chamar o método com um nome de arquivo como seu único argumento, o tipo de arquivo será considerado como RTF. Para especificar outro tipo de arquivo, chame o método com um valor da <xref:System.Windows.Forms.RichTextBoxStreamType> enumeração como seu segundo argumento.

    No exemplo a seguir, o <xref:System.Windows.Forms.OpenFileDialog> componente é mostrado quando um botão é clicado. O arquivo selecionado é então aberto e exibido no <xref:System.Windows.Forms.RichTextBox> controle. Este exemplo supõe que um formulário tem um botão, `btnOpenFile`.

    ```vb
    Private Sub btnOpenFile_Click(ByVal sender As System.Object, _
       ByVal e As System.EventArgs) Handles btnOpenFile.Click
         If OpenFileDialog1.ShowDialog() = DialogResult.OK Then
           RichTextBox1.LoadFile(OpenFileDialog1.FileName, _
              RichTextBoxStreamType.RichText)
          End If
    End Sub
    ```

    ```csharp
    private void btnOpenFile_Click(object sender, System.EventArgs e)
    {
       if(openFileDialog1.ShowDialog() == DialogResult.OK)
       {
         richTextBox1.LoadFile(openFileDialog1.FileName, RichTextBoxStreamType.RichText);
       }
    }
    ```

    ```cpp
    private:
       void btnOpenFile_Click(System::Object ^  sender,
          System::EventArgs ^  e)
       {
          if(openFileDialog1->ShowDialog() == DialogResult::OK)
          {
             richTextBox1->LoadFile(openFileDialog1->FileName,
                RichTextBoxStreamType::RichText);
          }
       }
    ```

    (Visual C#, Visual C++) Coloque o seguinte código no construtor do formulário para registrar o manipulador de eventos.

    ```csharp
    this.btnOpenFile.Click += new System.EventHandler(this. btnOpenFile_Click);
    ```

    ```cpp
    this->btnOpenFile->Click += gcnew
       System::EventHandler(this, &Form1::btnOpenFile_Click);
    ```

    > [!IMPORTANT]
    > Para executar esse processo, seu assembly pode exigir um nível de privilégio concedido pela <xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType> classe. Se você estiver executando em um contexto de confiança parcial, o processo poderá gerar uma exceção em razão dos privilégios insuficientes. Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](../../misc/code-access-security-basics.md).

## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.RichTextBox.LoadFile%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.RichTextBox>
- [Controle RichTextBox](richtextbox-control-windows-forms.md)
- [Controles a serem usados nos Windows Forms](controls-to-use-on-windows-forms.md)
