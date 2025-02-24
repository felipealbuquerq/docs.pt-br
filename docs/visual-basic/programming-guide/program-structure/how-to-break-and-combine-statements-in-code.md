---
title: 'Como: Instruções Break e combine no código (Visual Basic)'
ms.date: 07/20/2015
f1_keywords:
- vb._
helpviewer_keywords:
- colons (:)
- line continuation
- _ line-continuation character
- ': line separator character'
- Visual Basic code, line breaks in
- Visual Basic code, line breaks
- Visual Basic code, line continuation
- long lines of code
- line terminator
- line-continuation sequence
- underscores [Visual Basic], in code
- statements [Visual Basic], line continuation in
- line breaks [Visual Basic], in code
- line-continuation character [Visual Basic]
- Visual Basic code, line continuation in
- statements [Visual Basic], line breaks in
ms.assetid: dea01dad-a8ac-484a-bb3a-8c45a1b1eccc
ms.openlocfilehash: 745974523bd747dd23f3cfaf7cb70bb6cd4513f0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69946206"
---
# <a name="how-to-break-and-combine-statements-in-code-visual-basic"></a>Como: Instruções Break e combine no código (Visual Basic)
Ao escrever seu código, você pode ocasionalmente criar instruções demoradas que exigem a rolagem horizontal no editor de códigos. Embora isso não afete a maneira como seu código é executado, ele dificulta a leitura do código conforme ele aparece no monitor. Nesses casos, você deve considerar dividir a única instrução longa em várias linhas.  
  
### <a name="to-break-a-single-statement-into-multiple-lines"></a>Para dividir uma única instrução em várias linhas  
  
- Use o caractere de continuação de linha, que é um sublinhado (`_`), no ponto em que você deseja que a linha seja quebrada. O sublinhado deve ser imediatamente precedido por um espaço e imediatamente seguido por um terminador de linha (retorno de carro).  
  
    > [!NOTE]
    > Em alguns casos, se você omitir o caractere de continuação de linha, o compilador de Visual Basic irá continuar implicitamente a instrução na próxima linha de código. Para obter uma lista de elementos de sintaxe para os quais você pode omitir o caractere de continuação de linha, consulte "continuação de linha implícita" em [instruções](../../../visual-basic/programming-guide/language-features/statements.md).  
  
     No exemplo a seguir, a instrução é dividida em quatro linhas com caracteres de continuação de linha encerrando tudo menos a última linha.  
  
     [!code-vb[VbVbcnConventions#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#20)]  
  
     Usar essa sequência torna seu código mais fácil de ler, tanto online quanto quando impresso.  
  
     O caractere de continuação de linha deve ser o último caractere em uma linha. Você não pode acompanhá-lo com nada mais na mesma linha.  
  
     Existem algumas limitações para onde você pode usar o caractere de continuação de linha; por exemplo, você não pode usá-lo no meio de um nome de argumento. Você pode quebrar uma lista de argumentos com o caractere de continuação de linha, mas os nomes individuais dos argumentos devem permanecer intactos.  
  
     Você não pode continuar um comentário usando um caractere de continuação de linha. O compilador não examina os caracteres em um comentário para obter um significado especial. Para um comentário de várias linhas, repita o símbolo de comentário`'`() em cada linha.  
  
 Embora colocar cada instrução em uma linha separada seja o método recomendado, Visual Basic também permite colocar várias instruções na mesma linha.  
  
### <a name="to-place-multiple-statements-on-the-same-line"></a>Para posicionar várias instruções na mesma linha  
  
- Separe as instruções com dois-pontos`:`(), como no exemplo a seguir.  
  
     [!code-vb[VbVbcnConventions#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#10)]  
  
## <a name="see-also"></a>Consulte também

- [Estrutura do Programa e Convenções de Código](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
- [Instruções](../../../visual-basic/programming-guide/language-features/statements.md)
