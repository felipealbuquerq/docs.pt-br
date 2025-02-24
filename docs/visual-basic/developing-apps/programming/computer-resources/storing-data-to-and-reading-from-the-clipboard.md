---
title: Armazenando dados e lendo na Área de Transferência (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- Clipboard, storing data to (My.Computer.Clipboard)
- Clipboard, reading from (My.Computer.Clipboard)
- Clipboard
- My.Computer.Clipboard object, tasks
- data [Visual Basic], Clipboard
- reading data, from Clipboard
ms.assetid: f690119a-4378-4f7d-b20e-d9377ef49496
ms.openlocfilehash: d7693f6b5dc74e17686cd7d2667f32adbde9df80
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69916511"
---
# <a name="storing-data-to-and-reading-from-the-clipboard-visual-basic"></a>Armazenando dados e lendo na Área de Transferência (Visual Basic)
A Área de Transferência pode ser usada para armazenar dados, como texto e imagens. Como a Área de Transferência é compartilhada por todos os processos ativos, ela pode ser usada para transferir dados entre eles. O objeto `My.Computer.Clipboard` permite que você acesse facilmente a Área de Transferência e leia e grave nela.  
  
## <a name="reading-from-the-clipboard"></a>Lendo da Área de Transferência  
 Use o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetText%2A> para ler o texto da Área de Transferência. O código a seguir lê o texto e o mostra em uma caixa de mensagem. Deve haver texto armazenado na Área de Transferência para que o exemplo seja executado corretamente.  
  
 [!code-vb[VbVbcnMyClipboard#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#4)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No seletor de snippet de código, ele está localizado em **Aplicativos do Windows Forms &gt; Área de Transferência**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
 Use o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetImage%2A> para recuperar uma imagem da Área de Transferência. Este exemplo verifica se há alguma imagem na Área de Transferência antes de recuperá-la e atribuí-la à `PictureBox1`.  
  
 [!code-vb[VbResourceTasks#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#16)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No seletor de snippet de código, ele está localizado em **Aplicativos do Windows Forms &gt; Área de Transferência**. Para obter mais informações, consulte [Snippets de código](/visualstudio/ide/code-snippets).  
  
 Itens colocados na Área de Transferência persistirão mesmo após o aplicativo ser encerrado.  
  
## <a name="determining-the-type-of-file-stored-in-the-clipboard"></a>Determinando o tipo de arquivo armazenado na Área de Transferência  
 Os dados na área de transferência podem ter várias formas diferentes, como texto, um arquivo de áudio ou uma imagem. Para determinar que tipo de arquivo está na Área de Transferência, você pode usar métodos como <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsAudio%2A>, <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsFileDropList%2A>, <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsImage%2A> e <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsText%2A>. Você poderá usar o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsData%2A> se quiser verificar um formato personalizado.  
  
 Use a função `ContainsImage` para determinar se os dados contidos na Área de Transferência são uma imagem. O código a seguir verifica se os dados são uma imagem e relata de acordo.  
  
 [!code-vb[VbResourceTasks#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#13)]  
  
## <a name="clearing-the-clipboard"></a>Limpando a Área de Transferência  
 O método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.Clear%2A> limpa a Área de Transferência. Como a Área de Transferência é compartilhada por outros processos, limpá-la pode ter nesses processos.  
  
 O código a seguir mostra como usar o método `Clear`.  
  
 [!code-vb[VbVbcnMyClipboard#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#3)]  
  
## <a name="writing-to-the-clipboard"></a>Gravando na Área de Transferência  
 Use o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetText%2A> para gravar texto na Área de Transferência. O código a seguir grava a cadeia de caracteres "This is a test string" na Área de Transferência.  
  
 [!code-vb[VbVbcnMyClipboard#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#1)]  
  
 O método `SetText` pode aceitar um parâmetro de formato que contém um tipo de <xref:System.Windows.Forms.TextDataFormat>. O código a seguir grava a cadeia de caracteres "This is a test string" na Área de Transferência como um texto RTF.  
  
 [!code-vb[VbVbcnMyClipboard#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#2)]  
  
 Use o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetData%2A> para gravar dados na Área de Transferência. Este exemplo grava o `DataObject` `dataChunk` na área de transferência no formato personalizado `specialFormat`.  
  
 [!code-vb[VbVbcnMyClipboard#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#7)]  
  
 Use o método <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetAudio%2A> para gravar dados de áudio na Área de Transferência. Este exemplo cria a matriz de bytes `musicReader`, lê o arquivo `cool.wav` nela e o grava na Área de Transferência.  
  
 [!code-vb[VbResourceTasks#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#5)]  
  
> [!IMPORTANT]
> Como a Área de Transferência pode ser acessada por outros usuários, não a use para armazenar informações confidenciais, como senhas ou dados confidenciais.  
  
## <a name="see-also"></a>Consulte também

- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy>
- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetAudioStream%2A>
- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetDataObject%2A>
- [Como: Ler dados de objeto de um arquivo XML](../../../programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md)
- [Como: Gravar dados de objeto em um arquivo XML](../../../programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)
