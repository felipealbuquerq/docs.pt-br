---
title: Visão geral de constantes (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- constants [Visual Basic]
ms.assetid: 29016fe8-78b3-4dc8-90b8-1cfec2fa8ac9
ms.openlocfilehash: 0c866f3d03d26bd882d5a6596d40d1dc639da011
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69934775"
---
# <a name="constants-overview-visual-basic"></a>Visão geral de constantes (Visual Basic)
Uma constante é um nome significativo que substitui um número ou uma cadeia de caracteres que não é alterada. As constantes armazenam valores que, como o nome indica, permanecem os mesmos durante a execução de um aplicativo. Você pode melhorar muito a legibilidade do seu código e torná-lo mais fácil de manter usando constantes. Use-os no código que contém valores que reaparecem ou que dependem de determinados números que são difíceis de lembrar ou que não têm nenhum significado óbvio.  
  
## <a name="how-to-create-and-use-constants"></a>Como criar e usar constantes  
 Visual Basic contém um número de constantes predefinidas, principalmente usando para impressão e exibição. Você também pode criar suas próprias constantes com a `Const` instrução, usando as mesmas diretrizes que faria para criar um nome de variável. Se `Option Strict` for`On`, você deverá declarar explicitamente o tipo de constante.  
  
 O escopo de uma constante, que é o conjunto de todo o código que pode fazer referência a ele sem qualificar seu nome, é o mesmo de uma variável declarada no mesmo local. Para criar uma constante que existe dentro do escopo de um procedimento específico, declare-a dentro desse procedimento. Para criar uma constante que esteja disponível em um aplicativo, declare-a usando `Public` a palavra-chave na seção declarações da classe.  
  
> [!NOTE]
> Embora as constantes sejam semelhantes às variáveis, você não pode modificá-las ou atribuir novos valores a elas, como é possível para variáveis.  
  
 As constantes que você usa em seu código podem ser definidas pelo modelo de objeto para controles ou componentes com os quais você trabalha, ou podem ser definidas pelo usuário (ou seja, aquelas que você criar por conta própria).  
  
## <a name="compile-time-and-run-time-constants"></a>Constantes de tempo de compilação e tempo de execução  
 Uma constante de tempo de compilação é calculada no momento em que o código é compilado, enquanto uma constante de tempo de execução só pode ser computada enquanto o aplicativo está em execução. Uma constante de tempo de compilação terá o mesmo valor cada vez que um aplicativo for executado, enquanto uma constante de tempo de execução pode mudar a cada vez. Constantes de tempo de compilação são necessárias para casos como limites de matriz, expressões case ou inicializadores de enumeradores.  
  
## <a name="in-this-section"></a>Nesta seção  
  
|Definição|Termo|  
|---|---|  
|[Como: Declarar uma constante](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-a-constant.md)|Explica como usar a `Const` instrução para declarar uma constante e definir seu valor; ao declarar uma constante, você atribui um nome significativo ao valor.|  
|[Constantes Definidas pelo Usuário](../../../../visual-basic/programming-guide/language-features/constants-enums/user-defined-constants.md)|Descreve como criar suas próprias constantes, incluindo informações sobre o escopo e como evitar referências circulares.|  
|[Tipos de Dados Constante e Literal](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)|Fornece informações sobre como o compilador de Visual Basic inicializa constantes `Option Explicit` quando está desativado.|  
|[Como: Agrupar valores constantes relacionados](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-group-related-constant-values-together.md)|Demonstra como agrupar valores constantes relacionados.|  
  
## <a name="reference"></a>Referência  
  
|Definição|Termo|  
|---|---|  
|[Constantes e Enumerações](../../../../visual-basic/language-reference/constants-and-enumerations.md)|Lista as constantes predefinidas por Visual Basic.|  
|[Instrução Const](../../../../visual-basic/language-reference/statements/const-statement.md)|Descreve a `Const` instrução e seu uso.|  
|[Instrução Option Strict](../../../../visual-basic/language-reference/statements/option-strict-statement.md)|Descreve a `Option Strict` instrução e seu uso.|  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de Enumerações](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
- [Como: Inicializar uma variável de matriz no Visual Basic](../../../../visual-basic/programming-guide/language-features/arrays/how-to-initialize-an-array-variable.md)
