---
title: 'Como: Tornar um congelável somente leitura'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Freezable objects [WPF], making read-only
ms.assetid: 6c544b7d-d3c9-4736-aa90-4b8728234ccb
ms.openlocfilehash: 5748b7929db18578bbe00e3217b1578ac5fbc0f4
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64614597"
---
# <a name="how-to-make-a-freezable-read-only"></a>Como: Tornar um congelável somente leitura
Este exemplo mostra como fazer uma <xref:System.Windows.Freezable> somente leitura, chamando seu <xref:System.Windows.Freezable.Freeze%2A> método.  
  
 Você não é possível congelar um <xref:System.Windows.Freezable> do objeto se qualquer uma das seguintes condições for `true` sobre o objeto:  
  
- Ele tem propriedades animadas ou associadas a dados.  
  
- Ela tem propriedades que são definidas por um recurso dinâmico. Para obter mais informações sobre recursos dinâmicos, consulte o [recursos XAML](xaml-resources.md).  
  
- Ele contém <xref:System.Windows.Freezable> subobjetos não podem ser congelados.  
  
 Se essas condições forem `false` para seu <xref:System.Windows.Freezable> objeto e não pretende modificá-lo, considere congelá-lo para obter benefícios de desempenho.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir congela um <xref:System.Windows.Media.SolidColorBrush>, que é um tipo de <xref:System.Windows.Freezable> objeto.  
  
 [!code-csharp[freezablesample_procedural#FreezeExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#freezeexample1)]
 [!code-vb[freezablesample_procedural#FreezeExample1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#freezeexample1)]  
  
 Para obter mais informações sobre <xref:System.Windows.Freezable> objetos, consulte a [visão geral de objetos congeláveis](freezable-objects-overview.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Freezable>
- <xref:System.Windows.Freezable.CanFreeze%2A>
- <xref:System.Windows.Freezable.Freeze%2A>
- [Visão geral de objetos congeláveis](freezable-objects-overview.md)
- [Tópicos de instruções](base-elements-how-to-topics.md)
