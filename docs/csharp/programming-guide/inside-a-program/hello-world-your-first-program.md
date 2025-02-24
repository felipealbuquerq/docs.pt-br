---
title: Olá, Mundo – seu primeiro programa – Guia de programação em C#
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- cs.program
- vs.csharp.startpage.firstapplication
helpviewer_keywords:
- examples [C#], Hello World
- Hello World example [C#]
ms.assetid: 6493182a-b0b6-4539-a719-518a168cb730
ms.openlocfilehash: 9a50de0bb583a1dfccfa609be1cca732868505ba
ms.sourcegitcommit: 986f836f72ef10876878bd6217174e41464c145a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69589381"
---
# <a name="hello-world----your-first-program-c-programming-guide"></a>Olá, Mundo – seu primeiro programa (Guia de programação em C#)

O procedimento a seguir cria uma versão de C# do programa tradicional "Hello World!" programa. O programa exibe a cadeia de caracteres `Hello World!`

Para obter mais exemplos de conceitos introdutórios, consulte [Introdução ao Visual C# e ao Visual Basic](/visualstudio/ide/getting-started-with-visual-csharp-and-visual-basic).

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-and-run-a-console-application"></a>Para criar e executar um aplicativo de console

1. Inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Novo**, **Projeto**.

     A caixa de diálogo **Novo Projeto** é aberta.

3. Expanda **Instalado**, expanda **Modelos**, expanda **Visual C#** e, em seguida, escolha **Aplicativo de Console**.

4. Na caixa **Nome**, especifique um nome para o projeto e, em seguida, escolha o botão **OK**.

     O novo projeto aparece no **Gerenciador de Soluções**.

5. Se Program.cs não estiver aberto no **Editor de Códigos**, abra o menu de atalho para **Program.cs** no **Gerenciador de Soluções** e, em seguida, escolha **Exibir Código**.

6. Substitua o conteúdo de Program.cs pelo código a seguir.

     [!code-csharp[csProgGuide#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#21)]

7. Escolha a tecla F5 para executar o projeto. Uma janela do Prompt de Comando aparece, contendo a linha `Hello World!`

Em seguida, as partes importantes desse programa são examinadas.

## <a name="comments"></a>Comentários

A primeira linha contém um comentário. Os caracteres `//` convertem o restante da linha em um comentário.

 [!code-csharp[csProgGuide#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#32)]

Você também pode comentar um bloco de texto, colocando-o entre os caracteres `/*` e `*/`. Isso é mostrado no exemplo a seguir.

 [!code-csharp[csProgGuide#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#33)]

## <a name="main-method"></a>método Main

Um aplicativo de console do C# deve conter um método `Main`, no qual o controle começa e termina. O método `Main` é o local em que você cria objetos e executa outros métodos.

O método `Main` é um método [estático](../../language-reference/keywords/static.md) que reside dentro de uma classe ou um struct. No exemplo de "Hello World!" anterior, ele reside em uma classe chamada `Hello`. Você pode declarar o método `Main` de uma das seguintes maneiras:

- Ele pode retornar `void`.

     [!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

- Ele também pode retornar um inteiro.

     [!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

- Com qualquer um dos tipos de retorno, ele pode receber argumentos.

     [!code-csharp[csProgGuideMain#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#19)]

     -ou-

     [!code-csharp[csProgGuideMain#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#18)]

O parâmetro do método `Main`, `args`, é um matriz `string` que contém os argumentos de linha de comando usados para invocar o programa. Ao contrário do C++, a matriz não inclui o nome do arquivo executável (exe).

Para saber mais sobre como usar argumentos de linha de comando, confira os exemplos em [Main() e argumentos de linha de comando](../main-and-command-args/index.md) e [Como criar e usar assemblies usando a linha de comando](../concepts/assemblies-gac/how-to-create-and-use-assemblies-using-the-command-line.md).

A chamada para <xref:System.Console.ReadKey%2A> ao final do método `Main` impede que a janela de console seja fechada antes que você tenha a oportunidade de ler a saída, ao executar o programa no modo de depuração, pressionando F5.

## <a name="input-and-output"></a>Entrada e saída

Os programas em C# geralmente usam os serviços de entrada/saída fornecidos pela biblioteca em tempo de execução do .NET Framework. A instrução `System.Console.WriteLine("Hello World!");` usa o método <xref:System.Console.WriteLine%2A>. Esse é um dos métodos de saída da classe <xref:System.Console> na biblioteca em tempo de execução. Ele exibe seu parâmetro de cadeia de caracteres no fluxo de saída padrão, seguido por uma nova linha. Outros métodos <xref:System.Console> estão disponíveis para operações de saída e entrada diferentes. Se você incluir a diretiva `using System;` no início do programa, poderá usar diretamente as classes e métodos <xref:System> sem qualificá-los completamente. Por exemplo, você pode chamar `Console.WriteLine` em vez de `System.Console.WriteLine`:

 [!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

 [!code-csharp[csProgGuide#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#23)]

Para saber mais sobre os métodos de entrada/saída, veja <xref:System.IO>.

## <a name="command-line-compilation-and-execution"></a>Execução e compilação de linha de comando

Você pode compilar o programa "Hello World!" usando a linha de comando em vez do IDE (ambiente de desenvolvimento integrado) do Visual Studio.

### <a name="to-compile-and-run-from-a-command-prompt"></a>Para compilar e executar a partir de um prompt de comando

1. Cole o código do procedimento anterior em qualquer editor de texto e, em seguida, salve o arquivo como um arquivo de texto. Dê o nome `Hello.cs` para o arquivo. Os arquivos de código-fonte do C# usam a extensão `.cs`.

2. Realize uma das etapas a seguir para abrir uma janela de prompt de comando:

    - No Windows 10, no menu **Iniciar**, pesquise por `Developer Command Prompt` e, em seguida, toque ou escolha **Prompt de Comando do Desenvolvedor para VS 2017**.

         Uma janela do Prompt de Comando do Desenvolvedor será exibida.

    - No Windows 7, abra o menu **Iniciar**, expanda a pasta para a versão atual do Visual Studio, abra o menu de atalho para **Ferramentas do Visual Studio** e, em seguida, escolha **Prompt de Comando do Desenvolvedor para VS 2017**.

         Uma janela do Prompt de Comando do Desenvolvedor será exibida.

    - Habilitar builds de linha de comando em uma janela de Prompt de Comando padrão.

         Confira [Como configurar variáveis de ambiente para a linha de comando do Visual Studio](../../language-reference/compiler-options/how-to-set-environment-variables-for-the-visual-studio-command-line.md).

3. Na janela do prompt de comando, navegue até a pasta que contém seu arquivo `Hello.cs`.

4. Digite o seguinte comando para compilar `Hello.cs`.

     `csc Hello.cs`

     Se seu programa não tiver erros de compilação, um arquivo executável chamado `Hello.exe` será criado.

5. Na janela do prompt de comando, digite o seguinte comando para executar o programa:

     `Hello`

 Para obter mais informações sobre o compilador do C# e suas opções, consulte [Opções do compilador do C#](../../language-reference/compiler-options/index.md).

## <a name="see-also"></a>Consulte também

- [Guia de Programação em C#](../index.md)
- [Por dentro de um programa em C#](./index.md)
- [Cadeias de Caracteres](../strings/index.md)
- [Exemplos e tutoriais](../../../samples-and-tutorials/index.md)
- [Referência de C#](../../language-reference/index.md)
- [Main() e argumentos de linha de comando](../main-and-command-args/index.md)
- [Introdução ao Visual C# e ao Visual Basic](/visualstudio/ide/getting-started-with-visual-csharp-and-visual-basic)
