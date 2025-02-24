---
title: 'Como: Colocar objetos em camadas nos Windows Forms'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms controls, layering
- controls [Windows Forms], layering
- z order
- controls [Windows Forms], positioning
- z-order
ms.assetid: 1acc4281-2976-4715-86f4-bda68134baaf
author: gewarren
ms.author: gewarren
manager: jillfra
ms.openlocfilehash: 2e4c6a3236b3a2a2afaad73fee21c3cf59b992b8
ms.sourcegitcommit: 37616676fde89153f563a485fc6159fc57326fc2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69987557"
---
# <a name="how-to-layer-objects-on-windows-forms"></a>Como: Objetos de camada no Windows Forms

Ao criar uma interface do usuário complexa ou trabalhar com um formulário MDI (interface de múltiplos documentos), geralmente se deseja dispor em camadas controles e formulários filho para criar interfaces do usuário (UI) mais complexas. Para mover e acompanhar controles e janelas no contexto de um grupo, você manipula sua *ordem z*. A ordem Z é a camada visual de controles em um formulário ao longo do eixo z do formulário (profundidade). A janela na parte superior da ordem z se sobrepõe a todas as outras janelas. Todas as outras janelas se sobrepõe a janela na parte inferior da ordem z.

## <a name="to-layer-controls-at-design-time"></a>Dispondo controles em camadas no design time

1. No Visual Studio, selecione um controle que você deseja camada.

2. No menu **Formatar** , selecione **pedido**e, em seguida, selecione **trazer para frente** ou **Enviar para trás**.

## <a name="to-layer-controls-programmatically"></a>Dispondo controles em camadas de forma programática

Use os <xref:System.Windows.Forms.Control.BringToFront%2A> métodos <xref:System.Windows.Forms.Control.SendToBack%2A> e para manipular a ordem z dos controles.

Por exemplo, se um <xref:System.Windows.Forms.TextBox> controle, `txtFirstName`, estiver abaixo de outro controle e você quiser tê-lo na parte superior, use o seguinte código:

```vb
txtFirstName.BringToFront()
```

```csharp
txtFirstName.BringToFront();
```

```cpp
txtFirstName->BringToFront();
```

> [!NOTE]
> Windows Forms dá suporte a *Contenção de controle*. A contenção de controle envolve colocar um número de controles dentro de um controle recipiente, como um <xref:System.Windows.Forms.RadioButton> número de controles <xref:System.Windows.Forms.GroupBox> dentro de um controle. Em seguida, é possível dispor os controles em camadas no controle de contenção. Mover a caixa de grupo move também os controles, já que estes estão contidos dentro dela.

## <a name="see-also"></a>Consulte também

- [Controles dos Windows Forms](index.md)
- [Rotulando controles individuais dos Windows Forms e fornecendo atalhos para eles](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
- [Controles a serem usados nos Windows Forms](controls-to-use-on-windows-forms.md)
- [Controles dos Windows Forms por função](windows-forms-controls-by-function.md)
