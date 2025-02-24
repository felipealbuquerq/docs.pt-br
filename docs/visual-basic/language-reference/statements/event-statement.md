---
title: Instrução de evento (Visual Basic)
ms.date: 05/12/2018
f1_keywords:
- vb.Event
- vb.Custom
helpviewer_keywords:
- Event statement [Visual Basic]
- declaring events [Visual Basic], syntax
- Public keyword [Visual Basic], Event statements
- Custom keyword [Visual Basic]
- declarations [Visual Basic], events
- event keyword [Visual Basic]
- WithEvents keyword [Visual Basic], Event statements
- events [Visual Basic], declaring
- ByVal keyword [Visual Basic], Event statements
- events [Visual Basic], custom
- ByRef keyword [Visual Basic], Event statements
- declaring user-defined events
ms.assetid: 306ff8ed-74dd-4b6a-bd2f-e91b17474042
ms.openlocfilehash: 53d545a0c03749b13276a34f233f05493e5c94b0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69944432"
---
# <a name="event-statement"></a>Instrução Event
Declara um evento definido pelo usuário.  
  
## <a name="syntax"></a>Sintaxe  
  
```  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname[(parameterlist)] _  
[ Implements implementslist ]  
' -or-  
[ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Event eventname As delegatename _  
[ Implements implementslist ]  
' -or-  
 [ <attrlist> ] [ accessmodifier ] _  
[ Shared ] [ Shadows ] Custom Event eventname As delegatename _  
[ Implements implementslist ]  
   [ <attrlist> ] AddHandler(ByVal value As delegatename)  
      [ statements ]  
   End AddHandler  
   [ <attrlist> ] RemoveHandler(ByVal value As delegatename)  
      [ statements ]  
   End RemoveHandler  
   [ <attrlist> ] RaiseEvent(delegatesignature)  
      [ statements ]  
   End RaiseEvent  
End Event  
```  
  
## <a name="parts"></a>Partes  
  
|Parte|Descrição|  
|---|---|  
|`attrlist`|Opcional. Lista de atributos que se aplicam a este evento. Vários atributos são separados por vírgulas. Você deve colocar a [lista de atributos](../../../visual-basic/language-reference/statements/attribute-list.md) entre colchetes angulares`<`(""`>`e "").|  
|`accessmodifier`|Opcional. Especifica qual código pode acessar o evento. Pode ser um dos seguintes:<br /><br /> -   [Público](../../../visual-basic/language-reference/modifiers/public.md)— qualquer código que possa acessar o elemento que o declara pode acessá-lo.<br />-   [Protegido](../../../visual-basic/language-reference/modifiers/protected.md)– somente o código dentro de sua classe ou de uma classe derivada pode acessá-lo.<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)— somente o código no mesmo assembly pode acessá-lo.<br />-   [Particular](../../../visual-basic/language-reference/modifiers/private.md)— somente o código no elemento que o declara pode acessá-lo.<br /> -   Código somente [Friend protegido](../../language-reference/modifiers/protected-friend.md)na classe do evento, uma classe derivada ou o mesmo assembly pode acessá-lo. <br />- Código somente [protegido privado](../../language-reference/modifiers/private-protected.md)na classe do evento ou uma classe derivada no mesmo assembly pode acessá-lo.|  
|`Shared`|Opcional. Especifica que esse evento não está associado a uma instância específica de uma classe ou estrutura.|  
|`Shadows`|Opcional. Indica que esse evento redeclara e oculta um elemento de programação de nome idêntico, ou conjunto de elementos sobrecarregados, em uma classe base. Você pode sombrear qualquer tipo de elemento declarado com qualquer outro tipo.<br /><br /> Um elemento sombreado não está disponível de dentro da classe derivada que o sombreia, exceto de onde o elemento de sombreamento está inacessível. Por exemplo, se um `Private` elemento sombreia um elemento de classe base, o código que não tem permissão para acessar o `Private` elemento acessa o elemento de classe base em vez disso.|  
|`eventname`|Necessário. Nome do evento; segue as convenções padrão de nomenclatura de variável.|  
|`parameterlist`|Opcional. Lista de variáveis locais que representam os parâmetros deste evento. Você deve colocar a [lista de parâmetros](../../../visual-basic/language-reference/statements/parameter-list.md) entre parênteses.|  
|`Implements`|Opcional. Indica que esse evento implementa um evento de uma interface.|  
|`implementslist`|Necessário se `Implements` for fornecido. Lista de `Sub` procedimentos que estão sendo implementados. Vários procedimentos são separados por vírgulas:<br /><br /> *implementedprocedure* [ , *implementedprocedure* ... ]<br /><br /> Cada `implementedprocedure` uma tem a seguinte sintaxe e partes:<br /><br /> `interface`.`definedname`<br /><br /> -   `interface`Necessária. Nome de uma interface que o procedimento que contém a classe ou a estrutura está implementando.<br />-   `Definedname`Necessária. Nome pelo qual o procedimento é definido `interface`. Isso não precisa ser o mesmo `name`que o nome que este procedimento está usando para implementar o procedimento definido.|  
|`Custom`|Necessário. Eventos declarados como `Custom` devem definir `RemoveHandler`personalizados `AddHandler`, `RaiseEvent` e acessadores.|  
|`delegatename`|Opcional. O nome de um delegado que especifica a assinatura do manipulador de eventos.|  
|`AddHandler`|Necessário. Declara um `AddHandler` acessador, que especifica as instruções a serem executadas quando um manipulador de eventos é adicionado, seja explicitamente `AddHandler` usando a instrução ou implicitamente usando `Handles` a cláusula.|  
|`End AddHandler`|Necessário. Encerra o `AddHandler` bloco.|  
|`value`|Necessário. Nome do parâmetro.|  
|`RemoveHandler`|Necessário. Declara um `RemoveHandler` acessador, que especifica as instruções a serem executadas quando um manipulador de eventos é `RemoveHandler` removido usando a instrução.|  
|`End RemoveHandler`|Necessário. Encerra o `RemoveHandler` bloco.|  
|`RaiseEvent`|Necessário. Declara um `RaiseEvent` acessador, que especifica as instruções a serem executadas quando o evento é gerado `RaiseEvent` usando a instrução. Normalmente, isso invoca uma lista de delegados mantidos pelos `AddHandler` acessadores e. `RemoveHandler`|  
|`End RaiseEvent`|Necessário. Encerra o `RaiseEvent` bloco.|  
|`delegatesignature`|Necessário. Lista de parâmetros que corresponde aos parâmetros exigidos pelo `delegatename` delegado. Você deve colocar a [lista de parâmetros](../../../visual-basic/language-reference/statements/parameter-list.md) entre parênteses.|  
|`statements`|Opcional. Instruções que contêm os corpos dos `AddHandler`métodos, `RemoveHandler`e. `RaiseEvent`|  
|`End Event`|Necessário. Encerra o `Event` bloco.|  
  
## <a name="remarks"></a>Comentários  
 Depois que o evento tiver sido declarado, use `RaiseEvent` a instrução para gerar o evento. Um evento típico pode ser declarado e gerado conforme mostrado nos seguintes fragmentos:  
  
 [!code-vb[VbVbalrEvents#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#13)]  
  
> [!NOTE]
> Você pode declarar argumentos de evento da mesma forma como faz argumentos de procedimentos, com as seguintes exceções: eventos não podem ter `ParamArray` argumentos nomeados `Optional` , argumentos ou argumentos. Os eventos não têm valores de retorno.  
  
 Para lidar com um evento, você deve associá-lo a uma sub-rotina do manipulador de eventos `Handles` usando `AddHandler` a instrução ou. As assinaturas da sub-rotina e do evento devem corresponder. Para manipular um evento compartilhado, você deve usar a `AddHandler` instrução.  
  
 Você pode usar `Event` somente no nível do módulo. Isso significa que o *contexto de declaração* para um evento deve ser uma classe, estrutura, módulo ou interface e não pode ser um arquivo de origem, namespace, procedimento ou bloco. Para obter mais informações, consulte [Contextos de declaração e níveis de acesso padrão](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md).  
  
 Na maioria das circunstâncias, você pode usar a primeira sintaxe na seção sintaxe deste tópico para declarar eventos. No entanto, alguns cenários exigem que você tenha mais controle sobre o comportamento detalhado do evento. A última sintaxe na seção sintaxe deste tópico, que usa a `Custom` palavra-chave, fornece esse controle permitindo que você defina eventos personalizados. Em um evento personalizado, você especifica exatamente o que ocorre quando o código adiciona ou remove um manipulador de eventos de ou para o evento ou quando o código gera o evento. Para ver mais exemplos, veja [Como: Declare eventos personalizados para conservar memória](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md) e [como: Declare eventos personalizados para evitar o](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)bloqueio.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa eventos para contar os segundos de 10 a 0. O código ilustra vários dos métodos, propriedades e instruções relacionados ao evento. Isso inclui a `RaiseEvent` instrução.  
  
 A classe que gera um evento é a origem do evento, e os métodos que processam o evento são os manipuladores de eventos. Uma origem de evento pode ter vários manipuladores para os eventos que ele gera. Quando a classe gera o evento, esse evento é gerado em todas as classes que escolheram manipular eventos para essa instância do objeto.  
  
 O exemplo também usa um formulário (`Form1`) com um botão (`Button1`) e uma caixa de texto`TextBox1`(). Quando você clica no botão, a primeira caixa de texto exibe uma contagem regressiva de 10 a 0 segundos. Quando o tempo total (10 segundos) tiver decorrido, a primeira caixa de texto exibirá "concluído".  
  
 O código para `Form1` especifica os Estados inicial e de terminal do formulário. Ele também contém o código executado quando eventos são gerados.  
  
 Para usar este exemplo, abra um novo projeto Windows Forms. Em seguida, adicione um `Button1` botão chamado e uma caixa `TextBox1` de texto nomeada ao formulário principal `Form1`, denominada. Em seguida, clique com o botão direito do mouse no formulário e clique em **Exibir código** para abrir o editor de código.  
  
 Adicione uma `WithEvents` variável à seção `Form1` de declarações da classe:  
  
 [!code-vb[VbVbalrEvents#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#14)]  
  
 Adicione o código a seguir ao código para `Form1`. Substitua quaisquer procedimentos duplicados que possam existir, `Form_Load` como ou. `Button_Click`  
  
 [!code-vb[VbVbalrEvents#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#15)]  
  
 Pressione F5 para executar o exemplo anterior e clique no botão chamado **Iniciar**. A primeira caixa de texto começa a contar os segundos. Quando o tempo total (10 segundos) tiver decorrido, a primeira caixa de texto exibirá "concluído".  
  
> [!NOTE]
> O `My.Application.DoEvents` método não processa eventos da mesma maneira que o formulário. Para permitir que o formulário manipule os eventos diretamente, você pode usar multithreading. Para obter mais informações, consulte [Threading gerenciado](../../../standard/threading/index.md).  
  
## <a name="see-also"></a>Consulte também

- [Instrução RaiseEvent](../../../visual-basic/language-reference/statements/raiseevent-statement.md)
- [Instrução Implements](../../../visual-basic/language-reference/statements/implements-statement.md)
- [Eventos](../../../visual-basic/programming-guide/language-features/events/index.md)
- [Instrução AddHandler](../../../visual-basic/language-reference/statements/addhandler-statement.md)
- [Instrução RemoveHandler](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [Handles](../../../visual-basic/language-reference/statements/handles-clause.md)
- [Instrução Delegate](../../../visual-basic/language-reference/statements/delegate-statement.md)
- [Como: Declarar eventos personalizados para conservar a memória](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)
- [Como: Declarar eventos personalizados para evitar o bloqueio](../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)
- [Compartilhado](../../../visual-basic/language-reference/modifiers/shared.md)
- [Sombras](../../../visual-basic/language-reference/modifiers/shadows.md)
